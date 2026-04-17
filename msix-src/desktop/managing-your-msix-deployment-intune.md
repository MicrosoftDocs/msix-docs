---
description: This guide provides complete instructions for deploying MSIX apps to managed Windows devices using Microsoft Intune, including code signing, certificate trust, app creation, update management, and troubleshooting.
title: Deploy MSIX apps with Microsoft Intune
ms.date: 04/17/2026
ms.topic: how-to
keywords: windows 10, windows 11, intune, msix, lob app, code signing, certificate, deployment
---

# Deploy MSIX apps with Microsoft Intune

Microsoft Intune can deploy MSIX-packaged apps to managed Windows devices silently; no user interaction required at install time. This guide covers the full deployment workflow, including code signing, certificate trust distribution, app creation, assignment, and troubleshooting.

## Prerequisites

Before deploying an MSIX app through Intune, make sure you have:

- An MSIX package built and ready to deploy
- A Microsoft Intune subscription (included in Microsoft 365 E3/E5, Enterprise Mobility + Security)
- Microsoft Entra ID groups that represent the target users or devices
- A code signing certificate (see [Code signing requirements](#code-signing-requirements))

## Code signing requirements

Every MSIX package deployed via Intune must be signed. Windows will not install an unsigned MSIX package, even when deployed through MDM.

You have two options for signing certificates:

### Option 1: Self-signed certificate (no cost, for internal/managed devices only)

A self-signed certificate can sign your MSIX package at no cost, but the certificate must be trusted on every target device. Intune can distribute this trust automatically via a **Trusted Certificate** configuration profile.

This option is suitable when all target devices are Intune-managed and the app will never be distributed outside your organization.

### Option 2: Azure Trusted Signing (recommended for most scenarios)

[Azure Trusted Signing](/azure/trusted-signing/) provides a CA-trusted certificate with no hardware token required. Certificates integrate directly with CI/CD pipelines (GitHub Actions, Azure Pipelines). For current pricing, see the [Azure Trusted Signing pricing page](https://azure.microsoft.com/pricing/details/trusted-signing/).

Since the certificate is CA-trusted, Windows devices trust it automatically: no additional Intune configuration profiles needed. This is the lower-friction option for most enterprise scenarios.

For guidance on signing your MSIX package, see [Sign an app package using SignTool](/windows/msix/package/sign-app-package-using-signtool).

---

## Option A: Deploy with a self-signed certificate

If you're using a self-signed certificate, complete these steps **before** creating the LOB app. If using Azure Trusted Signing, skip to [Option B](#option-b-deploy-with-azure-trusted-signing).

### Step 1: Create and export your self-signed certificate

On your build machine, create a self-signed certificate and export it:

```powershell
# Create a self-signed code signing certificate
$cert = New-SelfSignedCertificate `
    -Subject "CN=MyCompany, O=MyCompany, C=US" `
    -Type CodeSigningCert `
    -CertStoreLocation Cert:\CurrentUser\My `
    -HashAlgorithm SHA256

# Export the public key (.cer) for Intune distribution — no password required
Export-Certificate -Cert $cert -FilePath "MyCompanyCert.cer"
```

### Step 2: Sign your MSIX package

Sign using the certificate from your local certificate store — no PFX or password needed on the build machine:

```powershell
# Sign using the certificate thumbprint
signtool sign /fd SHA256 /sha1 $cert.Thumbprint "MyApp.msix"
```

> [!TIP]
> If you need to sign on a different machine (for example in a CI/CD pipeline), export the PFX using `Export-PfxCertificate` and store the password as a pipeline secret variable — never hard-code it in a script or commit it to source control.

### Step 3: Create a Trusted Certificate profile in Intune

This profile pushes your certificate's public key to target devices so Windows trusts the self-signed signature.

1. In the [Intune admin center](https://intune.microsoft.com/), go to **Devices** > **Configuration** > **Create policy**
2. Select **Windows 10 and later** as platform, **Templates** as profile type, then **Trusted certificate**
3. Upload the `.cer` file you exported in Step 1
   4. Set **Destination store** to **Local Computer - Trusted People**
5. Assign the profile to the same device or user groups that will receive the app
6. Select **Create** — devices will receive the certificate within the next Intune sync cycle (typically within 15 minutes on active devices)

> [!IMPORTANT]
> Deploy the Trusted Certificate profile **before** or **at the same time** as the LOB app. If the app reaches the device before the certificate is trusted, installation will fail.

---

## Option B: Deploy with Azure Trusted Signing

If you're using [Azure Trusted Signing](/azure/trusted-signing/), the certificate is already trusted by Windows. No Trusted Certificate profile is needed in Intune.

Sign your MSIX package using the Azure Trusted Signing task in your CI/CD pipeline, or by following the local signing guidance in the [Azure Trusted Signing documentation](/azure/trusted-signing/).

---

## Create a Line-of-Business app in Intune

1. In the [Intune admin center](https://intune.microsoft.com/), go to **Apps** > **All apps** > **Add**
2. Under **App type**, select **Line-of-business app**, then select **Select**
3. Under **App package file**, upload your signed `.msix` (or `.msixbundle`) file
4. Intune reads the package metadata automatically — review the populated **Name**, **Publisher**, and **App version** fields
5. Complete the remaining required fields:
   - **Description** — what the app does
   - **Publisher** — your organization name (pre-populated from the package)
   - **App install context** — select **Device** for machine-wide install (recommended), or **User** for per-user install
6. Select **Next** to configure **Scope tags** if your organization uses them, then **Next** again
7. On the **Assignments** page, assign the app:
   - **Required** — app is installed silently with no user prompt
   - **Available for enrolled devices** — app appears in Company Portal for optional install
   - **Uninstall** — removes the app from targeted devices
8. Select **Next**, review your settings, then select **Create**

Intune queues the deployment. Devices receive and install the app during the next Intune sync cycle.

> [!NOTE]
> MSIX apps installed via Intune as **Required** install silently in the device context. Users do not see a UAC prompt or installation wizard.

## Verify installation

To confirm the app deployed successfully:

1. In the Intune admin center, go to **Apps** > **All apps** and select your app
2. Select **Device install status** or **User install status** to see per-device results
3. Look for **Installed** status. Common non-installed states:

| Status | Meaning |
|---|---|
| **Not applicable** | Device is not in the assigned group |
| **Pending** | Deployment queued; device hasn't synced yet |
| **Failed** | Installation error — see error code in details |
| **Not installed** | App not yet received or install deferred |

For failed installations, note the error code shown and see [Troubleshooting](#troubleshooting) below.

## Update management

When you publish a new version of your app, upload the updated MSIX package to the same app entry:

1. Go to **Apps** > **All apps**, select your app
2. Select **Properties** > **Edit** next to **App information**
3. Under **App package file**, upload the new `.msix` file
4. Save the changes — Intune detects the version change and pushes the update to assigned devices on next sync

> [!NOTE]
> Intune uses the **version number** in the MSIX manifest to detect updates. Make sure you increment the version number in your `.appxmanifest` before building each release.

## Troubleshooting

### Signature or certificate trust failure

If using a self-signed certificate, MSIX installation fails silently if the signing certificate isn't trusted on the device. To diagnose:

- In the Intune admin center, go to the device's **Configuration profiles** and verify the Trusted Certificate profile shows **Succeeded**
- On the device, open **certlm.msc** and verify the signing certificate appears in **Local Computer > Trusted People**
- If the profile is still **Pending**, trigger a manual sync (see [App stuck in "Pending"](#app-stuck-in-pending))

### 0x80073CF3 — Package failed update, dependency, or conflict validation

This error indicates `ERROR_INSTALL_PACKAGE_DOWNGRADE` — a newer version of the package is already installed on the device. Check:

- The version in the uploaded MSIX is higher than the version currently installed on target devices
- If needed, uninstall the existing package from the device before retrying the deployment
- For additional causes (dependency conflicts, framework mismatch), see [MSIX deployment troubleshooting](managing-your-msix-deployment-troubleshooting.md)

### 0x80073CF0 — Package could not be opened

The MSIX package could not be opened during install (`ERROR_INSTALL_OPEN_PACKAGE_FAILED`). Check:

- The package file was uploaded completely and is not corrupted
- The MSIX is valid — test by opening it locally before uploading
- Re-upload the package if the upload may have been interrupted, then retry the assignment

### 0x80070005 — Access denied

Indicates insufficient permissions (`E_ACCESSDENIED`). Verify that the deployment account and target device have the required access, and confirm the app and any required certificate profiles are assigned correctly. A context mismatch (app assigned as **User** install but the package requires machine-wide install) can also cause this — switch to **Device** install context if so. For additional causes and remediation steps, see [MSIX deployment troubleshooting](managing-your-msix-deployment-troubleshooting.md).

### App stuck in "Pending"

- Trigger a manual Intune sync: on the device, go to **Settings** > **Accounts** > **Access work or school** > select your account > **Info** > **Sync**
- Or from the Intune admin center, go to **Devices** > select the device > **Sync**

### Checking installation logs on the device

For deeper diagnostics, check two log sources:

**Intune Management Extension log** — tracks LOB app download and deployment activity:

```
%ProgramData%\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log
```

**AppxDeployment-Server event log** — tracks the MSIX installation itself:

1. Open **Event Viewer**
2. Go to **Applications and Services Logs** > **Microsoft** > **Windows** > **AppxDeployment-Server**
3. Filter for **Error** events and look for entries matching your package family name

Or from PowerShell:

```powershell
Get-AppxLog | Where-Object { $_.Message -match "MyApp" } | Select-Object TimeCreated, Message
```

Search both log sources for your app name or package family name to find the relevant installation attempt.

## Related content

- [Sign an app package using SignTool](/windows/msix/package/sign-app-package-using-signtool)
- [Azure Trusted Signing documentation](/azure/trusted-signing/)
- [Deploy MSIX apps with Configuration Manager](managing-your-msix-deployment-configmgr.md)
- [MSIX deployment troubleshooting](managing-your-msix-deployment-troubleshooting.md)
- [Create a Line-of-Business app in Intune (Microsoft docs)](/mem/intune/apps/lob-apps-windows)

