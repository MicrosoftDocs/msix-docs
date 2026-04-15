---
description: Solutions for the most common MSIX installation, signing, deployment, and runtime errors on Windows 10 and Windows 11.
title: MSIX troubleshooting guide
ms.date: 04/14/2026
ms.topic: troubleshooting-general
author: GrantMeStrength
ms.author: jken
keywords: windows, msix, troubleshooting, error, signing, deployment, app installer, vfs
---

# MSIX troubleshooting guide

This guide covers the most common MSIX errors across installation, signing, App Installer delivery, missing dependencies, and runtime behavior. Each section includes the symptom, root cause, and resolution.

For a full log of deployment events, open Event Viewer and navigate to:
**Applications and Services Logs → Microsoft → Windows → AppxDeployment-Server → Operational**

> [!TIP]
> For a consolidated set of diagnostics, also run [Windows App Certification Kit](/windows/uwp/debug-test-perf/windows-app-certification-kit) before distributing your package.

## Installation errors

### 0x80070005 — Access denied

**Symptom**: `Add-AppxPackage` or App Installer fails with error code `0x80070005`.

**Applies to**: Windows 10 and later, Windows 11

**Causes and fixes**:

| Cause | Fix |
|-------|-----|
| Running as a standard user without elevation when the package requires per-machine install | Run PowerShell as Administrator. To install for all users, use `Add-AppxProvisionedPackage` instead of `Add-AppxPackage`. |
| Antivirus or security software blocking the package file | Temporarily disable real-time scanning, or add an exclusion for the `.msix` / `.msixbundle` file |
| Package staged for another user and not provisioned | Use `Add-AppxProvisionedPackage` to provision for all users |
| File system ACLs blocking read access to the package | Check permissions on the package file with `icacls`; grant read access to the installing user |

### Package installation blocked because the app is in use

**Symptom**: Update or re-install fails with an error indicating the package is in use. In Event Viewer, you see that the deployment operation was rejected.

**Applies to**: Windows 10 and later, Windows 11

**Fix**: Close all running instances of the app before updating. If the app is a background service or has a background task registered, you may need to terminate those as well:

```powershell
Get-Process -Name "MyApp" | Stop-Process -Force
Add-AppxPackage -Path .\updated-app.msix
```

For enterprise deployments, consider scheduling updates during maintenance windows using Intune or Configuration Manager.

### MinVersion or architecture mismatch

**Symptom**: Installation fails with an error such as "The package could not be installed because it is not compatible with this version of Windows" or "The package is not applicable to this machine."

**Applies to**: Windows 10 and later

**Causes and fixes**:

| Cause | Fix |
|-------|-----|
| Package `MinVersion` in the manifest is higher than the OS version | Build a separate package targeting the installed OS version, or update the device |
| Architecture mismatch (e.g., arm64 package on x64 device) | Build and distribute the correct architecture variant; use a bundle (`.msixbundle`) to serve multiple architectures from one file |
| The package targets a Windows 11–only API without a compatibility check | Add a `TargetDeviceFamily` entry for both Windows 10 and Windows 11, or guard the API call with a version check at runtime |

> [!NOTE]
> Use `.msixbundle` files when distributing to mixed-architecture environments. A bundle contains packages for multiple architectures and Windows selects the correct one at install time.

### Add-AppxPackage succeeds but app is missing from Start Menu

**Symptom**: PowerShell reports success, but the app doesn't appear in the Start Menu or app list.

**Applies to**: Windows 10 and later, Windows 11

**Common causes**:

- **Per-user vs. per-machine install**: `Add-AppxPackage` installs for the current user only. If you're running as Administrator but expect the app for another user, use `Add-AppxProvisionedPackage` instead.
- **Package registered but tile not pinned**: The app is installed but Start Menu hasn't refreshed. Sign out and sign back in, or check **Settings → Apps** to confirm installation.
- **Missing Start Menu entry in the manifest**: Verify the `<Application>` element in `AppxManifest.xml` includes a `VisualElements` entry with a valid `Square150x150Logo`.
- **Duplicate package family name conflict**: If a newer or older version of the app is already installed with the same package family name, the new install may silently replace it. Check with `Get-AppxPackage -Name "YourPackageFamilyName"`.

## Signing and certificate errors

> [!NOTE]
> For detailed SignTool error codes and flags, see [Known issues and troubleshooting for SignTool](package/signing-known-issues.md).

### Certificate not trusted (0x800B0109)

**Symptom**: Installation fails with error `0x800B0109` — "A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider."

**Applies to**: Windows 10 and later, Windows 11

**Cause**: The certificate used to sign the package is not in the device's trusted certificate stores. This is common when using self-signed certificates for development.

**Fix**: Import the signing certificate into the device's **Local Computer → Trusted People** store (not the current user store — App Installer only checks the machine store):

```powershell
# Export the certificate from the package first (if needed)
$cert = (Get-AuthenticodeSignature -FilePath .\app.msix).SignerCertificate
Export-Certificate -Cert $cert -FilePath .\app-cert.cer

# Import into Trusted People (requires administrator rights)
Import-Certificate -FilePath .\app-cert.cer -CertStoreLocation Cert:\LocalMachine\TrustedPeople
```

> [!IMPORTANT]
> Do not import signing certificates into the **Trusted Root Certification Authorities** store unless the certificate is a root CA. Importing untrusted self-signed certs there weakens the device's security posture.

For production apps, use a certificate issued by a trusted CA or [Azure Artifact Signing (formerly Trusted Signing)](/azure/trusted-signing/), which chains to the Microsoft Identity Verification Root Certificate Authority — trusted by default on Windows 10 version 1809 and later, Windows 11.

### Publisher name mismatch (0x8007000B, Event ID 150)

**Symptom**: SignTool fails with `0x8007000B`, and Event Viewer (AppxPackagingOM operational log) shows **Event ID 150**:

```
error 0x8007000B: The app manifest publisher name (CN=Contoso) must match
the subject name of the signing certificate (CN=Contoso, C=US).
```

**Applies to**: Windows 10 and later, Windows 11

**Cause**: The `Publisher` attribute in `AppxManifest.xml` must exactly match the **subject name** of the signing certificate — including all distinguished name fields in the same order.

**Fix**: 

1. Get the exact subject name from the certificate:
   ```powershell
   (Get-Item Cert:\CurrentUser\My\THUMBPRINT).Subject
   # Example output: CN=Contoso, C=US
   ```

2. Update `AppxManifest.xml` to match exactly:
   ```xml
   <Identity Name="Contoso.MyApp"
             Publisher="CN=Contoso, C=US"
             ... />
   ```

3. Re-package with `MakeAppx.exe` and re-sign.

> [!TIP]
> When using Azure Artifact Signing (formerly Trusted Signing), the publisher value in your manifest must match your verified identity — found in the Azure portal under your certificate profile's **Subject name** field.

## App Installer and web delivery errors

### Schema version mismatch in .appinstaller file

**Symptom**: App Installer fails to parse or process the `.appinstaller` file, often with a generic error about an invalid file or unsupported schema.

**Applies to**: Windows 10 and later (version-dependent — see table)

**Cause**: The `Uri` attribute of the `<AppInstaller>` root element specifies a schema version that the installed version of Windows doesn't support.

**Schema versions by Windows release**:

| Windows version | Minimum supported schema version |
|-----------------|----------------------------------|
| Windows 10 version 1709 | 1.0.0.0 |
| Windows 10 version 1803 | 1.1.0.0 |
| Windows 10 version 1809 | 1.2.0.0 |
| Windows 10 version 1903 and later, Windows 11 | 1.3.0.0, 1.4.0.0, 1.5.0.0 |

**Fix**: Set the schema URI in your `.appinstaller` file to the minimum version your lowest-supported OS requires:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller Uri="https://example.com/app.appinstaller"
              Version="1.0.0.0"
              xmlns="http://schemas.microsoft.com/appx/appinstaller/2017">
```

Avoid using the latest schema version if you need to support older Windows 10 versions.

### Files served with incorrect MIME type or missing Content-Length

**Symptom**: App Installer fails when installing from an HTTP/HTTPS endpoint. The error may be generic, such as `0x80072F76` ("Unknown error") or "App installation failed."

**Applies to**: Windows 10 and later

**Cause**: The web server is serving the `.msix`, `.msixbundle`, or `.appinstaller` file with an incorrect `Content-Type` header, or omitting the `Content-Length` header.

**Fix**: Configure your web server to serve MSIX-related files with the correct MIME types:

| File extension | Required MIME type |
|----------------|--------------------|
| `.msix` | `application/msix` |
| `.msixbundle` | `application/msixbundle` |
| `.appinstaller` | `application/appinstaller` |
| `.appx` | `application/appx` |
| `.appxbundle` | `application/appxbundle` |

Also ensure every response includes a valid `Content-Length` header — this applies to both `GET` and `HEAD` requests.

For IIS, add MIME type mappings in `web.config`. For Azure Static Web Apps or GitHub Pages, MIME types for these extensions may need explicit configuration or a custom hosting solution.

## Missing dependencies

### Framework package not installed (VCLibs, .NET, Windows App SDK)

**Symptom**: The app installs successfully but crashes on launch, or installation fails with a dependency error referencing a package family name like `Microsoft.VCLibs`, `Microsoft.WindowsAppRuntime`, or `Microsoft.NET.Native`.

**Applies to**: Windows 10 and later, Windows 11

**Common frameworks and where to get them**:

| Framework | Needed when | Source |
|-----------|-------------|--------|
| VCLibs (x64/x86/arm64) | App uses C++ runtime | Microsoft Store (auto-installed) or [direct download](/troubleshoot/developer/visualstudio/cpp/libraries/c-runtime-packages-desktop-bridge) |
| .NET 8 Desktop Runtime | App targets .NET 8 | Included with Windows App SDK; or [direct download](https://dotnet.microsoft.com/download) |
| Windows App SDK (WinAppSDK) | App uses WinUI 3 or other WinAppSDK APIs | [Windows App SDK releases on GitHub](https://github.com/microsoft/WindowsAppSDK/releases) |

**Fix for development**: When installing via `Add-AppxPackage` locally, add the `-DependencyPackages` parameter or install the framework packages first:

```powershell
# Install VCLibs dependency first
Add-AppxPackage -Path .\Microsoft.VCLibs.x64.14.00.Desktop.appx

# Then install your app
Add-AppxPackage -Path .\MyApp.msix
```

**Fix for distribution**: If distributing outside the Store, include framework packages in your `.appinstaller` file under `<Dependencies>`:

```xml
<Dependencies>
  <Package Name="Microsoft.VCLibs.140.00.UWPDesktop"
           Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
           Version="14.0.30704.0"
           Uri="https://aka.ms/Microsoft.VCLibs.x64.14.00.Desktop.appx"
           ProcessorArchitecture="x64"/>
</Dependencies>
```

> [!NOTE]
> When distributing through the **Microsoft Store**, framework dependencies are automatically downloaded and installed. Manual dependency management is only needed for sideloading and enterprise deployments.

## SignTool not found in CI/CD

**Symptom**: CI/CD pipeline (GitHub Actions, Azure DevOps) fails with an error like `'signtool' is not recognized as an internal or external command` or `SignTool.exe: not found`.

**Applies to**: Windows 10 and later, Windows 11 (signing machine)

**Cause**: SignTool is part of the Windows SDK and is not included in standard CI runner images by default.

**Fixes**:

**Option 1 — Install Windows SDK in the pipeline** (GitHub Actions):
```yaml
- name: Install Windows SDK
  run: |
    winget install --id Microsoft.WindowsSDK.10.0.22621 --accept-source-agreements --accept-package-agreements
```

**Option 2 — Use the WinApp CLI** (simplest for MSIX signing):
```yaml
- name: Install WinApp CLI
  run: winget install -e --id Microsoft.WinAppCLI --source winget --accept-source-agreements
- name: Sign MSIX
  run: winapp sign output\MyApp.msix --cert ${{ secrets.CERT_THUMBPRINT }}
```

**Option 3 — Use Azure Artifact Signing** (recommended for production):
```yaml
- name: Sign with Azure Artifact Signing
  uses: azure/trusted-signing-action@v0
  with:
    azure-tenant-id: ${{ secrets.AZURE_TENANT_ID }}
    azure-client-id: ${{ secrets.AZURE_CLIENT_ID }}
    azure-client-secret: ${{ secrets.AZURE_CLIENT_SECRET }}
    endpoint: ${{ secrets.AZURE_ARTIFACT_SIGNING_ENDPOINT }}
    trusted-signing-account-name: ${{ secrets.AZURE_CODE_SIGNING_NAME }}
    certificate-profile-name: ${{ secrets.AZURE_CERT_PROFILE_NAME }}
    files-folder: ${{ github.workspace }}\output
    files-folder-filter: msix
```

> [!NOTE]
> The GitHub Action is named `azure/trusted-signing-action` (the former service name). This is the official action regardless of the rebranding to Artifact Signing.

For a full walkthrough of CI/CD signing setup, see [Sign your MSIX package - end-to-end guide](package/sign-msix-package-guide.md).

## Runtime and virtualization behavior

MSIX packages run inside a lightweight app container. Some behaviors that appear to be bugs are actually by design — the container intercepts file and registry operations to protect the rest of the system.

### Why file writes seem to disappear (VFS file redirection)

**Symptom**: The app writes a file to a path like `C:\Program Files\MyApp\config.ini` at runtime, but the file doesn't appear there. The app reads the value back correctly, but other processes or users don't see it.

**Applies to**: Windows 10 and later, Windows 11

**Explanation**: MSIX uses **Virtual File System (VFS)** redirection. Writes to protected system paths are silently redirected to a per-user container:

```
%LocalAppData%\Packages\<PackageFamilyName>\LocalCache\Local\VFS\
```

This is by design — it prevents MSIX apps from modifying shared system locations, supporting clean uninstall.

**Options**:
- **Use app data folders instead**: Write to `ApplicationData.Current.LocalFolder` (WinRT) or `%LocalAppData%\Packages\<PFN>\LocalState\` for per-user data that should persist.
- **Use `AppData\Roaming`** for data that should roam across devices.
- **Inspect the VFS container** to see redirected files: `%LocalAppData%\Packages\<PackageFamilyName>\LocalCache\`

For more detail on what paths are virtualized, see [Troubleshoot runtime issues in an MSIX container](manage/troubleshoot-msix-container.md).

### Why registry writes seem to disappear (registry virtualization)

**Symptom**: The app writes to `HKEY_LOCAL_MACHINE\Software\MyApp\` at runtime, but the value isn't visible to other processes or survives a reinstall.

**Applies to**: Windows 10 and later, Windows 11

**Explanation**: MSIX intercepts writes to `HKLM\Software` and redirects them to a per-package registry hive that is isolated from the rest of the system. On uninstall, the hive is deleted.

**Options**:
- Write per-user settings to `HKEY_CURRENT_USER\Software\<AppName>` — these are **not** virtualized and persist.
- Use the Windows [ApplicationData APIs](/uwp/api/windows.storage.applicationdata) for structured settings storage.
- Declare registry entries that must be shared across users or processes in `AppxManifest.xml` using the `<Extensions>` / `<com:Extension>` mechanism.

For a deeper look at MSIX container behavior, see [Understand how packaged desktop apps run on Windows](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes).

## Windows 10 MSIX limitations

Some MSIX features require Windows 11 or a specific Windows 10 version. If you're deploying to Windows 10 devices, be aware of the following.

### Features that require Windows 10, version 2004 (build 19041) or later

| Feature | Minimum version |
|---|---|
| Auto-update 2021 schema (`ShowPrompt`, `UpdateBlocksActivation`) | Windows 10 2004 (19041) |
| Package integrity enforcement (`uap10:PackageIntegrity`) | Windows 10 2004 (19041) |
| App Installer auto-repair | Windows 10 2004 (19041) |
| No separate sideloading policy toggle for trusted app package installs; see [Enable your device for development](/windows/apps/get-started/developer-mode-features-and-debugging) for current requirements | Windows 10 2004 (19041) |

### Features that require Windows 11

| Feature | Notes |
|---|---|
| Sparse package identity without full packaging | Requires Windows 11 |
| MSIX support for unpackaged apps with external location (full platform support) | Some APIs improved in Windows 11 |
| Improved per-machine MSIX install performance | Windows 11 optimizations |

### Windows 10 devices older than version 1709

MSIX is not natively supported on Windows 10 versions prior to 1709 (Fall Creators Update). To deploy MSIX packages to those devices, use [MSIX Core](msix-core/msixcore.md), which provides a compatibility layer for down-level Windows 10 versions.

### Manifest extensions

Some `AppxManifest.xml` namespace extensions are Windows 11-only. Declaring them on a package targeting Windows 10 may fail schema validation during packaging or be rejected during installation. Check the [App package manifest schema reference](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) for the `MinOSVersion` listed for each extension.

### Debugging tip

To verify what MSIX features are available on a specific Windows 10 build, check the [MSIX features and supported platforms](supported-platforms.md) page, which lists feature availability by OS version.

## Related resources

- [Sign an MSIX package overview](package/signing-package-overview.md)
- [Sign your MSIX package - end-to-end guide](package/sign-msix-package-guide.md)
- [Known issues and troubleshooting for SignTool](package/signing-known-issues.md)
- [Troubleshoot App Installer issues](app-installer/troubleshoot-appinstaller-issues.md)
- [Troubleshoot runtime issues in an MSIX container](manage/troubleshoot-msix-container.md)
- [Troubleshooting packaging, deployment, and query of Windows apps](/windows/win32/appxpkg/troubleshooting)
