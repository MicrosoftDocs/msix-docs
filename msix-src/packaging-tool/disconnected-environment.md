---
title: Using the MSIX Packaging Tool in a disconnected environment
description: This article describes how to acquire all of the assets required for the MSIX Packaging Tool if you are in a disconnected environment.
ms.date: 08/26/2026
ms.topic: concept-article
keywords: msix
---

# Using the MSIX Packaging Tool in a disconnected environment

The MSIX Packaging Tool is available from the Microsoft Store, but conversion devices in restricted networks might not have Store or internet access. This article explains how to transfer and install the public release of the tool and its driver on a disconnected device. It doesn't apply to [Insider program](insider-program.md) releases.

## Get the MSIX Packaging Tool

On an internet-connected device, download the offline bundle and license:

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Download 1.2024.405.0 MSIX Packaging Tool](https://download.microsoft.com/download/e/2/e/e2e923b2-7a3a-4730-969d-ab37001fbb5e/MSIXPackagingtoolv1.2024.405.0.msixbundle)
> [!div class="button" style="text-align: left;" width="150px;"] 
> [Offline copy of the license](https://download.microsoft.com/download/e/2/e/e2e923b2-7a3a-4730-969d-ab37001fbb5e/MSIXPackagingtoolv1.2024.405.0.License.xml)

Copy both files to the disconnected device. Keep the license with the matching bundle version.

## Install the tool on a disconnected device

To provision the tool on the running Windows installation, open PowerShell as an administrator and run:

```powershell
Add-AppxProvisionedPackage -Online `
    -PackagePath "C:\MSIX\MSIXPackagingtoolv1.2024.405.0.msixbundle" `
    -LicensePath "C:\MSIX\MSIXPackagingtoolv1.2024.405.0.License.xml"
```

The `-Online` parameter means the currently running Windows installation; it doesn't require an internet connection. For the complete command reference, see [Add-AppxProvisionedPackage](/powershell/module/dism/add-appxprovisionedpackage).

Provisioning registers the package for new user profiles at sign-in. If the tool doesn't appear for an existing signed-in user, sign out and sign in again, or register the package for that user with `Add-AppxPackage`.

To provision the tool into a mounted Windows image instead, replace `-Online` with `-Path` and specify the root of the mounted image:

```powershell
Add-AppxProvisionedPackage -Path "C:\MountedImage" `
    -PackagePath "C:\MSIX\MSIXPackagingtoolv1.2024.405.0.msixbundle" `
    -LicensePath "C:\MSIX\MSIXPackagingtoolv1.2024.405.0.License.xml"
```

Use a version of DISM that is the same as or newer than the Windows image that you service. For more information, see [DISM supported platforms](/windows-hardware/manufacture/desktop/dism-supported-platforms).

## Install the MSIX Packaging Tool driver

The MSIX Packaging Tool driver is delivered as a [Feature on Demand (FOD)](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities). On a connected device, the tool can acquire the driver from Windows Update. Driver acquisition fails if the Windows Update service is disabled. On Windows Insider builds, the selected flight and the device build must also match the available FOD.

For a disconnected device, obtain the Features on Demand media designated for the device's Windows release and architecture, as shown in the following table. FOD packages are serviced Windows components; some media covers a range of releases (for example, the Windows 10, version 2004 Features on Demand ISO applies to version 2004 and later). Match the architecture exactly, and don't substitute media that the table doesn't list for the device's release.

| Conversion device | Offline source |
| --- | --- |
| Windows 11 | Matching Windows 11 Languages and Optional Features ISO |
| Windows 10, version 2004 and later | Windows 10, version 2004 Features on Demand ISO |

The [Features on Demand media table](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#features-on-demand-media) identifies the media for other Windows releases. Acquire the ISO through a channel available to your organization:

- Volume licensing customers can [download volume licensing products](/microsoft-365/commerce/licenses/download-vl-products) from the Microsoft 365 admin center.
- Visual Studio subscribers can use [Visual Studio Subscriptions Downloads](https://my.visualstudio.com/Downloads/Featured).
- OEMs and system builders can obtain media through the [Microsoft OEM site](https://go.microsoft.com/fwlink/?LinkId=131359) or [Device Partner Center](https://devicepartner.microsoft.com/).

Mount the matching ISO and locate the `Msix-PackagingTool-Driver-Package` CAB for the device architecture. In an elevated Command Prompt, install that package on the running Windows installation:

```cmd
DISM /Online /Add-Package /PackagePath:"D:\<FOD repository path>\Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab"
```

Replace `D:` and the example path with the location on the mounted media. Use the x86 CAB instead of the amd64 CAB for an x86 device. If you create a reduced repository instead of using the mounted ISO, follow the [FOD repository guidance](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#fod-repositories) so the repository includes the required metadata and dependencies.

The direct-download driver CABs formerly listed in this article targeted Windows releases that are out of support. For supported releases, use matching FOD media and review the [Windows lifecycle FAQ](/lifecycle/faq/windows) before servicing an older conversion device.
