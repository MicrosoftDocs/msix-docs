---
title: Using the MSIX Packaging Tool in a disconnected environment
description: This article describes how to acquire all of the assets required for the MSIX Packaging Tool if you are in a disconnected environment.
ms.date: 10/03/2023
ms.topic: article
keywords: msix
---

# Using the MSIX Packaging Tool in a disconnected environment

While we make it super easy for users to acquire the MSIX Packaging Tool through the Microsoft Store, we know that not everyone has access to the Store or to a connected environment where they want to perform conversions. So this topic is about using the tool in a disconnected mode. The info here applies only to our public releases; not to our [Insider program](insider-program.md) releases.

## Get the MSIX Packaging Tool

You can download the latest version of the offline package below.

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Download 1.2023.319.0 MSIX Packaging Tool](https://download.microsoft.com/download/d/0/0/d0043667-b1db-4060-9c82-eaee1fa619e8/MsixPackagingToolv1.2023.319.0.msixbundle)

If you encounter issues with the offline copy of the packaging tool, then download the offline copy of the license for the tool below.

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Offline copy of the license](https://download.microsoft.com/download/d/0/0/d0043667-b1db-4060-9c82-eaee1fa619e8/MsixPackagingToolv1.2023.319.0_License.xml)

After you have the offline version of the tool, you can use [PowerShell](/powershell/module/dism/add-appxprovisionedpackage&preserve-view=true) to add the app package and license to your machine.

### Example of offline installation

```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

The MSIX Packaging Tool driver is delivered as a [Feature on Demand (FOD)](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) package from Windows Update, and it will fail to install if the Windows Update service is disabled on the machine, or if Windows Insider flight ring settings don't match the operating system (OS) build number of the computer.

If you're in an enterprise environment with Windows Server Update Services (WSUS) or Systems Center (now Microsoft Endpoint Manager), then you might need to modify your default configuration (see [How to make Features on Demand and language packs available when you're using WSUS or Configuration Manager](/windows/deployment/update/fod-and-lang-packs)). Or just download and install the FOD manually:

- Download the FOD .cab file for [Windows 10, version 1809, x64](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 1809, x86](https://download.microsoft.com/download/9/9/4/9948d09d-af25-45a5-b01f-cc4bcf05f5bf/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab)
- Download the FOD .cab file for [Windows 10, version 1903, x64](https://download.microsoft.com/download/5/2/e/52ec35e9-3b50-47b2-879d-c815a93bc3fc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) or [Windows 10, version 1903, x86](https://download.microsoft.com/download/2/c/3/2c3a78a2-4d64-426a-976d-dfe4805110cc/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~x86~~.cab) **NOTE: This will also work for Windows 10, version 1909**
- Download the FOD .cab file for [Windows 10, version 2004, x64](https://download.microsoft.com/download/4/c/7/4c79bf31-946c-444a-bc5f-61398d3b0a76/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab) **NOTE: This will also work for later Windows 10 versions**
- Individually-obtained Feature on Demand (FOD) packages can be installed using [DISM command-line options](/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). In an elevated PowerShell window type: ```Dism /Online /add-package /packagepath:(path)``` **NOTE: Please ensure that the file path contains the right file name with the '.cab' extension.**

IT admins can also create [Side by side feature store (shared folder)](/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) to allow access to the MSIX Packaging tool driver FOD. You can find additional details at the bottom of the blog post [Language pack acquisition and retention for enterprise devices](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Otherwise, if you have access to enterprise or OEM channels, then you can obtain the driver from Windows 10 Features on Demand media from one of the following sources:

- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): Volume License access is required.
- [OEM Portal](https://www.microsoftoem.com): OEM access is required.
- [MSDN Download](https://my.visualstudio.com/Downloads/Featured): MSDN subscription is required.
