---
title: Using the MSIX Packaging Tool in a disconnected environment
description: This article describes how to acquire all of the assets required for the MSIX Packaging Tool if you are in a disconnected environment.
ms.date: 02/05/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Using the MSIX Packaging Tool in a disconnected environment

While we make it super easy for users to acquire the MSIX Packaging Tool through the Microsoft store, but we know that not everyone has the store, or a connected environment where they want to perform conversions. This is only available for our public releases, not for our [Insider program](insider-program.md) releases.

## Get the MSIX Packaging Tool

The MSIX Packaging Tool can be downloaded for offline use in the enterprise from the Microsoft Store for Business [web portal](https://businessstore.microsoft.com/store). You can learn more about offline distribution [here](/microsoft-store/distribute-offline-apps).

If you are encountering issues with the offline copy of the packaging tool, make sure you have the [offline copy of the license](/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) for the tool. 

After you have the offline version of the application, you can use [PowerShell](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) to add the app package and license to your machine.

#### Example of offline installation
```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

## Get the MSIX Packaging Tool driver

The MSIX Packaging Tool driver is delivered as a [Feature on Demand (FOD)](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) package from Windows Update and will fail to install if the Windows Update service is disabled on the machine or Windows Insider flight ring settings do not match the OS build of the computer.

If you are in an enterprise environment with Windows Server Update Services (WSUS) or Systems Center (now Microsoft Endpoint Manager) you may need to modify your [default configuration](/windows/deployment/update/fod-and-lang-packs), or just download and install the FOD manually:

- Download the FOD .cab file for [Windows 10, version 1809, x64](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 1809, x86](https://download.microsoft.com/download/9/9/4/9948d09d-af25-45a5-b01f-cc4bcf05f5bf/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Download the FOD .cab file for [Windows 10, version 1903, x64](https://download.microsoft.com/download/5/2/e/52ec35e9-3b50-47b2-879d-c815a93bc3fc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 1903, x86](https://download.microsoft.com/download/2/c/3/2c3a78a2-4d64-426a-976d-dfe4805110cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab) **NOTE: This will also work for Windows 10, version 1909**
- Download the FOD .cab file for [Windows 10, version 2004, x64](https://download.microsoft.com/download/a/f/1/af16ad00-3b28-4c8a-9765-1e14a21e93d2/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 2004, x86](https://download.microsoft.com/download/e/5/7/e57f0cec-807b-403e-9ac8-abb2799d09e5/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Download the FOD .cab file for [Windows 10, version 20H2, x64](https://download.microsoft.com/download/a/a/d/aadf507e-5fc1-4ae9-92c2-e1634e895a0b/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 20H2, x86](https://download.microsoft.com/download/9/f/f/9ffdbe5b-b317-4bf0-91a3-aa8c283082cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Individually-obtained Feature on Demand (FOD) packages can be installed using [DISM command-line options](/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). In an elevated PowerShell window type: ```Dism /Online /add-package /packagepath:(path)```

IT admins can also create [Side by side feature store (shared folder)](/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) to allow access to the MSIX Packaging tool driver FOD. You can find additional details at the bottom of this [blog post](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Otherwise, if you have access to enterprise or OEM channels you can obtain the driver from Windows 10 Features on Demand media from one of the following sources:

- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): Volume License access is required.
- [OEM Portal](https://www.microsoftoem.com): OEM access is required.
- [MSDN Download](https://my.visualstudio.com/Downloads/Featured): MSDN subscription is required.
