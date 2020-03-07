---
title: Deploy MSIX Core apps with Microsoft Endpoint Configuration Manager
description: Describes how to deploy MSIX Core with Microsoft Endpoint Configuration Manager.
ms.date: 03/02/2020
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Deploy MSIX Core apps with Microsoft Endpoint Configuration Manager
The delivery of MSIX applications using the Microsoft Endpoint Configuration Manager allows IT Pros to link other applications as dependencies, forcing them to install prior to. By creating a dependency to the MSIX Core application, we enforce the MSIX Core application to be installed only when required by the device. For more information on application dependencies in Micosoft Endpoint Configuration Manager see: [Create Applications: Deployment type Dependencies](https://docs.microsoft.com/configmgr/apps/deploy-use/create-applications#bkmk_dt-depend).

## Get started
The following steps will guide you through setting up an MSIX Core deployment strategy by using Microsoft Endpoint Configuration Manager:

1. [Deploy MSIX Core with Microsoft Endpoint Configuration Manager](deploy-msix-core-with-configmgr.md)
1. [Update your existing MSIX package to support MSIX Core](support-msix-core.md)
1. Deploy MSIX Core Apps with Microsoft Endpoint Configuration Manager

## Creating the MSIX Core Microsoft Endpoint Configuration Manager application
The following will guide you through the creation of a Microsoft Endpoint Configuration Manager application, for the purpose of deploying MSIX Core apps to client devices.
 
Assuming you followed the previous guides (See the list of guides in the Get Started section above) and have retrieved/updated/created an MSIX Core enlightened app. As well as have copied the app to a file share that is accessible by the Microsoft Endpoint Configuration Manager tool. The next step is to deploy the new app to client devices within your environment.

### Create MSIX Core dependent application in Microsoft Endpoint Configuration Manager
1. From within the Microsoft Endpoint Configuration Manager console navigate to: **Software Library > Overview / Application Management / Applications**.
1. Select **Create Application** from the ribbon.
1. Select the **Manually specify the application information** radio button.
1. Select the **Next** button.
1. Enter the application details into the appropriate fields.
1. Select the **Next** button twice.
1. Select the **Add** button.
1. Set the Type to **Script Installer**.
1. Select the **Next** button.
1. Enter the application name with a suffix of " **- MSIXCore**" (IE: "Application Y - MSIXCore").
1. Select the **Next** button.
1. Select the **Browse** button next to Content location and navigate to the file share containing the app installation media.
1. Select the **Select Folder** button.
1. Select the **Browse** button next to Installation Program, set the file type to be **All Files ( * . * )** and select the installation media.
1. Select the **Open** button.
1. Update the Installation program field to: 
```batch
"C:\Program Files\msixmgr\msixmgr.exe -AddPackage [Application.msix] -quietUX"
```
1. Set the Uninstall program field to be: 
```batch
"C:\Program Files\msixmgr\msixmgr.exe" -RemovePackage [Application Family Name] -quietUX
```
1. Replace [Application Family Name] with the application family name of the MSIX application.
1. Select the **Next** button.
1. Select the **Use a custom script to detect the presence of this deployment type** radio button.
1. Select the **Edit** button.
1. Verify that Script type is set as **PowerShell**
1. Enter the following: 
```PowerShell
Set-Location "C:\Program Files\msixmgr"

IF([Boolean]$(get-item "msixmgr.exe"))
{
    $Result = $(.\msixmgr.exe -FindPackage [Application Family Name]*)

    IF($($Result.GetType().Name) -eq "Object[]")
    {
        Return 1
    }
}
```
1. Update [Application Family Name] with the MSIX package family name of the application.
1. Select the **Ok** button.
1. Select the **Next** button.
1. Set the Installation behavior to **Install for User**.
1. Set the Maximum allowed run time (minutes) and Estimated installation time (minutes) to values approriate for this application.
1. Set the Installation program visibility as **Hidden**.
1. Select the **Next** button.
1. Select the **Add** button.
1. Ensure the Category has been set to **Device**.
1. Set the Condition as **Operating system**
1. Select the **Windows 7** checkbox from the list of Operating systems.
1. Select the **Ok** button.
1. Select the **Next** button.
1. Select the **Add** button.
1. Set the Dependency group name as **MSIX Core**.
1. Select the **Add** button.
1. Select **MSIX Core** from the list of Available applications.
1. Select both 32-bit and 64-bit options from the Deployment types list.
1. Select the **Ok** button.
1. Select the **Ok** button.
1. Select the **Next** button twice.
1. Select the **Close** button.

### Add non-MSIX Core dependent deployment type
1. Select the **Add** button.
1. Ensure the Type has been set to **Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)**. 
1. Select the **Browse...** button and navigate to the MSIX Core enlighted application installation media, then select the **Open** button.
1. Select the **Next** button six times.
1. Select the **Close** button.
1. Select the **Next** button twice.
1. Select the **Close** button.
