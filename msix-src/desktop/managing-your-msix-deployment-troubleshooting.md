---
description: Troubleshooting guidance for MSIX installation errors, including a reference table of common error codes, causes, and fixes.
title: MSIX deployment troubleshooting
ms.date: 04/15/2026
ms.topic: troubleshooting-general
keywords: windows 10, deployment, msix, error code, troubleshooting
ms.assetid:  
---

# MSIX deployment troubleshooting

This article helps you diagnose and resolve MSIX installation and deployment failures. It covers common error codes, how to read deployment logs, and fixes for the most frequently reported issues.

> [!NOTE]
> References to `.msix` in this article apply equally to `.msixbundle`, `.appx`, `.appxbundle`, and their encrypted variants (`.emsix`, `.eappx`, and so on) unless noted otherwise.

## Step 1: Read the deployment logs

When an MSIX install fails, the error code shown in the UI is often a high-level summary. The Event Viewer and PowerShell logs contain the underlying cause.

### Event Viewer

Open Event Viewer and navigate to:
`Applications and Services Logs > Microsoft > Windows > AppxDeployment-Server`

Look for events with a level of **Critical**, **Error**, or **Warning** that match the time of the failed install. The detail pane usually contains a more specific error code and description than the one shown to the user.

### PowerShell

```powershell
Get-AppxLog | Where-Object {$_.EventId -eq 404} | Select-Object -Last 20
```

Or for an interactive, filterable view:

```powershell
Get-AppxLog | Out-GridView
```

## Step 2: Check common error codes

The following table covers the error codes most frequently reported by developers. If your code isn't listed, see [Troubleshooting packaging, deployment, and query of Windows apps](/windows/win32/appxpkg/troubleshooting) for a full reference.

| Error code | Name | Common cause | Fix |
|---|---|---|---|
| **0x80073CF0** | `ERROR_INSTALL_OPEN_PACKAGE_FAILED` | Package file can't be opened — missing file, wrong path, or permissions issue | Verify the `.msix` file exists at the specified path. If installing via PowerShell, use a local path, not a UNC/network share path. Run PowerShell with admin privileges. |
| **0x80073CF3** | `ERROR_INSTALL_PACKAGE_DOWNGRADE` | A newer version of the package is already installed | Uninstall the existing version first, or change the version number in your package manifest to a higher value. |
| **0x80073CF6** | `ERROR_INSTALL_REGISTRATION_FAILURE` | Package registration failed — often a corrupted package registration database or system file | Run `wsreset -i` to reset the Microsoft Store cache. If the problem persists, run `sfc /scannow` to repair system files, or try **Settings > Installed apps > [App name] > Advanced options > Repair**. Check Event Viewer for a more specific inner error. |
| **0x80073CF9** | `ERROR_INSTALL_PACKAGE_NOT_FOUND` | The package or a dependency package could not be found during installation | Ensure all dependency packages (VCLibs, .NET, WinAppSDK runtime) are installed before the main package. You can install them in the same command: `Add-AppxPackage main.msix -DependencyPath dep1.msix, dep2.msix` |
| **0x80073CFA** | `ERROR_REMOVE_FAILED` | Uninstall/removal operation failed — the app may be running, its state may be corrupted, or it is a protected inbox app | Close all instances of the app and try again. For corrupted state, try in a Clean Boot environment. System/inbox apps cannot be removed via `Remove-AppxPackage`. |
| **0x80073CFB** | `ERROR_PACKAGE_ALREADY_EXISTS` | Identical package already registered for this user | Uninstall the existing package first: `Get-AppxPackage <PackageName> | Remove-AppxPackage` |
| **0x80073D02** | `ERROR_PACKAGES_IN_USE` | The package is currently in use by a running process | Close all running instances of the app before updating or uninstalling. Use Task Manager or `Get-Process` to confirm. |
| **0x8007000D** | `ERROR_INVALID_DATA` | Package is corrupt, badly formatted, or the publisher in the manifest doesn't match the signing certificate | Verify the `Publisher` field in `AppxManifest.xml` (or `Package.appxmanifest` in Visual Studio) exactly matches the **Subject** of your signing certificate (including spacing and capitalization). Rebuild and re-sign the package. |
| **0x8BAD0042** | `CertNotTrusted` | The signing certificate is not trusted by the device (commonly seen when using MSIX Core on down-level Windows) | Import the signing certificate into the **Local Computer > Trusted People** store (not the current user store) on the target device, or use a certificate issued by a trusted CA. See [Trusted certificates](../app-installer/troubleshoot-appinstaller-issues.md#trusted-certificates). |
| **0x80070005** | `E_ACCESSDENIED` | Insufficient permissions | Run the install command or script with admin privileges (for example, from a command prompt opened with **Run as administrator**). |
| **0x80070002** | `ERROR_FILE_NOT_FOUND` | A file referenced in the manifest doesn't exist in the package | Check for missing assets or resources referenced in `AppxManifest.xml`. Rebuild the package and verify its contents with `MakeAppx unpack`. |

> [!TIP]
> **Installing from a network share?** A common cause of misleading errors (especially 0x80073CF0 and 0x80070002) is running `Add-AppxPackage` against a UNC path (`\\server\share\app.msix`). Copy the `.msix` file to a local folder first, then install.

## Step 3: Verify prerequisites

Many install failures are caused by unmet prerequisites rather than a problem with the package itself.

**Certificate trust** — The package signing certificate must be in the **Local Computer > Trusted People** store, not the current user's certificate store.

**Sideloading** — On Windows 10 versions before 1809 (RS5), sideloading required a separate policy setting. From RS5 onward the default allows trusted app package installs; on Windows 11, no additional setting is required. See [Enable your device for development](/windows/apps/get-started/developer-mode-features-and-debugging) for current requirements.

**Dependencies** — Windows App SDK apps require the [Windows App SDK runtime](/windows/apps/windows-app-sdk/downloads) to be installed on the target device unless you're using [self-contained deployment](/windows/apps/package-and-deploy/self-contained-deploy/deploy-self-contained-apps). Use the `-DependencyPath` parameter with `Add-AppxPackage` to install dependency packages together with the main package.

**App Installer app** — If double-clicking an `.msix` file does nothing, the App Installer app may be missing. Install it from the [Microsoft Store](https://apps.microsoft.com/detail/9NBLGGH4NNS1).

## Step 4: Validate your package manifest

Manifest errors are a common source of installation failures. Use `MakeAppx` to verify the package structure and check the manifest:

```powershell
# Unpack the package and inspect the manifest
MakeAppx unpack /p "C:\path\to\app.msix" /d "C:\unpack-output"
```

Check `AppxManifest.xml` in the output folder for:
- XML validity (no malformed tags or missing closing elements)
- `Publisher` field matching your signing certificate's **Subject** exactly
- Capability names using the correct namespace for your target OS version
- `MinOSVersion` constraints for any Windows 11-only extensions

See the [App package manifest schema reference](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) for supported attributes and namespaces.

## Common DEP error codes

Visual Studio and MSBuild surface deployment errors with a `DEP` prefix. The most common ones:

| Code | Meaning | Fix |
|---|---|---|
| **DEP0700** | Package registration failed | Usually a manifest schema error — check for invalid XML, unsupported `xmlns` declarations, or capability names that don't match the allowed list. |
| **DEP3300** | Dependency not found | Install the required framework package (VCLibs, .NET, WinAppSDK) before deploying. |
| **DEP3301** | Package already registered with different architecture | Uninstall the conflicting architecture before deploying a different architecture build. |

## Additional resources

- [Troubleshooting packaging, deployment, and query of Windows apps (full error code reference)](/windows/win32/appxpkg/troubleshooting)
- [Troubleshoot App Installer file issues](../app-installer/troubleshoot-appinstaller-issues.md)
- [MSIX Packaging Tool known issues](../packaging-tool/tool-known-issues.md)
- [Run, debug, and test a packaged desktop application](./desktop-to-uwp-debug.md)

Have questions? Ask [GitHub Copilot](https://copilot.github.com) or search Stack Overflow using the [msix](https://stackoverflow.com/questions/tagged/msix) tag.
