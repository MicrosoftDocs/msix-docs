---
description: End-to-end guide to signing MSIX packages for development, testing, and production distribution, including Azure Artifact Signing and CI/CD integration.
title: Sign your MSIX package - end-to-end guide
ms.date: 07/02/2026
ms.topic: how-to
keywords: windows, msix, signing, certificate, artifact signing, trusted signing, signtool, winapp cli, sideloading, ci/cd, msbuild, visual studio
---

# Sign your MSIX package: end-to-end guide

This guide walks you through signing an MSIX package for each stage of development — from local testing through production distribution. For a comparison of signing options and costs, see [Sign an MSIX package overview](signing-package-overview.md).

## Choose a signing approach

| Stage | Recommended approach | Windows version |
|---|---|---|
| Local development and testing | WinApp CLI (self-signed cert) | Windows 10 and later |
| Distribute to testers (sideloading) | Self-signed cert with cert trust step | Windows 10 and later |
| Production distribution | Azure Artifact Signing (formerly Trusted Signing) | Windows 10 version 1809 and later, Windows Server 2016 and later |
| Microsoft Store distribution | Signed by the Store on submission | All supported Windows versions |

---

## Development: Sign for local testing

For local development, use a self-signed certificate. Self-signed packages can only be installed on machines where the certificate is explicitly trusted — this is intentional and appropriate for testing.

### Option A: WinApp CLI (recommended for new projects)

> [!IMPORTANT]
> The Windows App Development CLI is currently in **public preview** and requires Windows 10 with [WinGet](/windows/package-manager/winget/) installed.

The [WinApp CLI](/windows/apps/dev-tools/winapp-cli) handles certificate generation and signing in a single step.

**Step 1: Install the WinApp CLI**

```powershell
winget install -e --id Microsoft.WinAppCLI --source winget
```

**Step 2: Generate a self-signed development certificate**

```powershell
winapp cert generate --manifest .\appxmanifest.xml --output .\devcert.pfx --install
```

The `--manifest` flag reads the publisher name directly from your `appxmanifest.xml`. The `--install` flag adds the certificate to the local machine trust store, so you can immediately install your package.

**Step 3: Sign the package**

```powershell
winapp sign MyApp.msix --cert .\devcert.pfx
```

### Option B: PowerShell + SignTool (Windows 10 and later)

Use this approach if you're not using the WinApp CLI.

**Step 1: Create a self-signed certificate**

Run the following in an elevated PowerShell prompt. The `Subject` value must exactly match the `Publisher` in your `appxmanifest.xml`:

```powershell
New-SelfSignedCertificate -Type Custom -KeyUsage DigitalSignature `
  -Subject "CN=MyPublisher" `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}") `
  -FriendlyName "MyApp Dev Cert"
```

Note the thumbprint in the output — you'll need it in the next steps.

**Step 2: Export the certificate to a PFX file**

```powershell
$password = ConvertTo-SecureString -String "YourPassword" -Force -AsPlainText
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Thumbprint>" `
  -FilePath .\devcert.pfx -Password $password
```

**Step 3: Trust the certificate locally**

```powershell
Import-PfxCertificate -CertStoreLocation "Cert:\LocalMachine\TrustedPeople" `
  -FilePath .\devcert.pfx -Password $password
```

**Step 4: Sign the package**

```powershell
SignTool sign /fd SHA256 /a /f .\devcert.pfx /p "YourPassword" MyApp.msix
```

For full SignTool usage, see [Sign an app package using SignTool](sign-app-package-using-signtool.md).

---

## Automate signing in your Visual Studio build (MSBuild)

Instead of signing as a separate manual step, you can have Visual Studio&mdash;or a command-line `msbuild` build on a CI agent&mdash;sign the package automatically every time you build. This is the modern replacement for older post-build signing scripts.

Windows Application Packaging Projects (`.wapproj`) and single-project MSIX apps expose MSBuild properties that control build-time signing. Set them in the project file, or pass them on the `msbuild` command line.

| MSBuild property | Description |
|---|---|
| `AppxPackageSigningEnabled` | Set to `true` to sign the package during the build. |
| `PackageCertificateThumbprint` | Thumbprint of a certificate in the current user's **Personal** store (`Cert:\CurrentUser\My`) to sign with. |
| `PackageCertificateKeyFile` | Path to a `.pfx` file to sign with, when the certificate isn't installed in the store. |
| `PackageCertificatePassword` | Password for the `.pfx` file, if it's password-protected. |
| `AppxPackageSigningTimestampServerUrl` | Timestamp server URL. Timestamping is strongly recommended so the signature stays valid after the certificate expires. |
| `AppxPackageSigningTimestampDigestAlgorithm` | Timestamp digest algorithm. Use `SHA256`. |

> [!NOTE]
> Specify **either** `PackageCertificateThumbprint` (a certificate in the store) **or** `PackageCertificateKeyFile` (a `.pfx` file)&mdash;not both. On a CI agent, prefer installing the certificate to the store and referencing it by thumbprint so you don't pass a password on the command line.

**Sign at build time from the command line or a CI agent** using a certificate installed in the store:

```cmd
msbuild MyApp.wapproj /p:Configuration=Release /p:Platform=x64 ^
  /p:AppxPackageSigningEnabled=true ^
  /p:PackageCertificateThumbprint=<thumbprint> ^
  /p:AppxPackageSigningTimestampServerUrl=http://timestamp.digicert.com ^
  /p:AppxPackageSigningTimestampDigestAlgorithm=SHA256
```

Or using a `.pfx` file:

```cmd
msbuild MyApp.wapproj /p:Configuration=Release /p:Platform=x64 ^
  /p:AppxPackageSigningEnabled=true ^
  /p:PackageCertificateKeyFile=.\devcert.pfx ^
  /p:PackageCertificatePassword=<password> ^
  /p:AppxPackageSigningTimestampServerUrl=http://timestamp.digicert.com
```

> [!TIP]
> To sign automatically inside Visual Studio, configure the signing certificate once on the packaging project's **Package.appxmanifest** > **Packaging** tab. Visual Studio saves the equivalent properties in the project file, so every subsequent build signs the package.

### Post-build signing with Azure Artifact Signing or Azure Key Vault

The `PackageCertificate*` properties above use a local certificate or `.pfx` file. When you sign with **Azure Artifact Signing** (formerly Trusted Signing) or a certificate stored in **Azure Key Vault**, the private key never leaves the service, so sign as a **post-build step** instead:

1. Build the package **unsigned** by setting `/p:AppxPackageSigningEnabled=false`.
2. Sign the produced `.msix` as a subsequent step:
   - For Azure Artifact Signing, run SignTool with the dlib plugin&mdash;see [Production: Azure Artifact Signing](#production-azure-artifact-signing-formerly-trusted-signing).
   - For Azure Key Vault from Visual Studio, see [Sign packages with Azure Key Vault](../desktop/sign-with-akv-cert.md).

---

## Testing: Distribute to testers with a self-signed certificate

To install a self-signed MSIX package on a tester's machine, the certificate must be trusted on that machine first.

> [!NOTE]
> On **Windows 10 version 2004 and later** and **Windows 11**, sideloading is enabled by default. On earlier Windows 10 versions, testers must enable **Sideload apps** in **Settings > Update & Security > For developers**.

**Give testers the certificate:** Share the `.pfx` or `.cer` (public key only) file alongside the `.msix` package.

**Testers run the following in an elevated PowerShell prompt:**

```powershell
# If you shared a .pfx file (testers need the password)
Import-PfxCertificate -CertStoreLocation "Cert:\LocalMachine\TrustedPeople" `
  -FilePath .\devcert.pfx -Password (ConvertTo-SecureString "YourPassword" -Force -AsPlainText)

# If you shared a .cer file (public key only, no password needed)
Import-Certificate -CertStoreLocation "Cert:\LocalMachine\TrustedPeople" -FilePath .\devcert.cer
```

After trusting the certificate, testers can install the `.msix` by double-clicking it.

> [!IMPORTANT]
> Self-signed certificates should only be used for testing. Remove them from tester machines when no longer needed. For broad distribution, use a publicly trusted signing method instead.

---

## Production: Azure Artifact Signing (formerly Trusted Signing)

**Applies to:** Windows 10 version 1809 and later, Windows 11 (all versions), Windows Server 2016 and later

Azure Artifact Signing is the new name for what was previously called Trusted Signing. The service is identical — only the name changed. You may still see "Trusted Signing" in some tooling (such as the `azure/trusted-signing-action` GitHub Action and the `winget` package ID) as those references are being updated over time.

Azure Artifact Signing is the recommended option for production MSIX signing. Reputation is tied to your verified identity rather than a specific certificate, which means your publisher identity builds SmartScreen reputation over time as users install your signed apps. See [Sign an MSIX package overview](signing-package-overview.md) for eligibility requirements and cost information before setting up an account.

> [!IMPORTANT]
> **Azure Artifact Signing availability:** Organizations in the USA, Canada, the European Union, and the United Kingdom can sign up. Individual developers are currently limited to the USA and Canada. If you're an individual developer outside those regions, use an OV code signing certificate from a CA instead (see [Production: OV code signing certificate](#production-ov-code-signing-certificate) below).

> [!NOTE]
> Signing with Azure Artifact Signing does **not** provide instant SmartScreen trust. Like OV certificates, your apps will initially show a SmartScreen warning until your publisher identity builds sufficient download reputation — typically several weeks and hundreds of clean installs. This is expected behavior for new publishers. For details on how SmartScreen reputation works and what to expect as a new publisher, see [SmartScreen reputation for Windows app developers](/windows/apps/package-and-deploy/smartscreen-reputation).

### Prerequisites

- An Artifact Signing account with identity validation completed and a certificate profile created. See the [Artifact Signing quickstart](/azure/trusted-signing/quickstart).
- The **Trusted Signing Certificate Profile Signer** role assigned to the identity used for signing.

### Step 1: Install Artifact Signing Client Tools

The Artifact Signing Client Tools include the required dlib plugin, a compatible version of SignTool, and the .NET 8 runtime. Standard SignTool syntax does **not** work with Artifact Signing without this package.

```powershell
winget install -e --id Microsoft.Azure.ArtifactSigningClientTools
```

If WinGet isn't available (for example on a Windows Server build agent), install via PowerShell as administrator:

```powershell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri "https://download.microsoft.com/download/70ad2c3b-761f-4aa9-a9de-e7405aa2b4c1/ArtifactSigningClientTools.msi" -OutFile .\ArtifactSigningClientTools.msi
Start-Process msiexec.exe -Wait -ArgumentList '/I ArtifactSigningClientTools.msi /quiet'
Remove-Item .\ArtifactSigningClientTools.msi
```

### Step 2: Create a metadata JSON file

Create a file named `metadata.json` with your Artifact Signing account details. The endpoint URI must match the Azure region where you created your account (find it in the Azure portal under your Artifact Signing account as **Account URI**):

```json
{
  "Endpoint": "https://<region>.codesigning.azure.net/",
  "CodeSigningAccountName": "<your-account-name>",
  "CertificateProfileName": "<your-certificate-profile-name>"
}
```

For the full list of region-specific endpoint URIs, see [Signing integrations](/azure/trusted-signing/how-to-signing-integrations#create-a-json-file).

### Step 3: Authenticate

Set your Azure credentials as environment variables (use App Registration credentials for CI/CD):

```powershell
$env:AZURE_CLIENT_ID     = "<your-client-id>"
$env:AZURE_TENANT_ID     = "<your-tenant-id>"
$env:AZURE_CLIENT_SECRET = "<your-client-secret>"
```

Alternatively, run `az login` for interactive local signing.

### Step 4: Sign the package

Use the SignTool installed with the Artifact Signing Client Tools. The `/dlib` flag points to the dlib plugin installed in the previous step:

```powershell
signtool sign /v /fd SHA256 `
  /tr "https://timestamp.acs.microsoft.com" /td SHA256 `
  /dlib "C:\Program Files (x86)\Microsoft\ArtifactSigningClientTools\bin\Azure.CodeSigning.Dlib.dll" `
  /dmdf .\metadata.json `
  MyApp.msix
```

> [!TIP]
> Use the `/debug` flag to get detailed output if signing fails — it shows certificate chain and authentication details.

### CI/CD: GitHub Actions

Use the official [`azure/trusted-signing-action`](https://github.com/azure/trusted-signing-action):

```yaml
- name: Sign MSIX with Azure Artifact Signing
  uses: azure/trusted-signing-action@v0
  with:
    azure-tenant-id: ${{ secrets.AZURE_TENANT_ID }}
    azure-client-id: ${{ secrets.AZURE_CLIENT_ID }}
    azure-client-secret: ${{ secrets.AZURE_CLIENT_SECRET }}
    endpoint: ${{ secrets.AZURE_TRUSTED_SIGNING_ENDPOINT }}
    trusted-signing-account-name: ${{ secrets.AZURE_CODE_SIGNING_NAME }}
    certificate-profile-name: ${{ secrets.AZURE_CERT_PROFILE_NAME }}
    files-folder: ${{ github.workspace }}\output
    files-folder-filter: msix
```

For Azure DevOps, see [Set up Azure DevOps with Trusted Signing](/azure/trusted-signing/how-to-signing-integrations#set-up-azure-devops-with-trusted-signing).

---

## Production: Microsoft Store distribution

**Applies to:** All supported Windows versions

If you're distributing through the Microsoft Store, you don't need to sign the package yourself — the Store signs it during the submission process. Build your package and submit it via [Partner Center](https://partner.microsoft.com/dashboard). The Store provides a globally trusted signature and handles all certificate management.

---

## Production: OV code signing certificate

**Applies to:** Windows 10 and later

If Artifact Signing isn't available for your region or situation, you can purchase an OV (Organization Validation) code signing certificate from a certificate authority (CA) such as DigiCert, Sectigo, or GlobalSign. Costs typically range from $300–500/year.

Once you have a PFX file from your CA:

```powershell
SignTool sign /fd SHA256 /a /f .\cert.pfx /p "YourPassword" `
  /tr http://timestamp.digicert.com /td SHA256 `
  MyApp.msix
```

For full signing options, see [Sign an app package using SignTool](sign-app-package-using-signtool.md).

> [!NOTE]
> OV certificates don't provide instant SmartScreen trust. Reputation builds over time as your signed packages are installed without being flagged. For details, see [SmartScreen reputation for Windows app developers](/windows/apps/package-and-deploy/smartscreen-reputation).

---

## Related articles

- [Sign an MSIX package overview](signing-package-overview.md)
- [Sign an app package using SignTool](sign-app-package-using-signtool.md)
- [Create a certificate for package signing](create-certificate-package-signing.md)
- [Sign packages with Azure Key Vault](../desktop/sign-with-akv-cert.md)
- [Artifact Signing quickstart](/azure/trusted-signing/quickstart)
- [WinApp CLI reference](/windows/apps/dev-tools/winapp-cli)
- [MSIX troubleshooting guide](../msix-troubleshooting-guide.md)
