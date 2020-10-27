---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing MSIX with PowerShell
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix, PowerShell, PSH, PS, PoSh, cmdlets
ms.assetid:  
ms.localizationpriority: medium
---

# Managing MSIX with PowerShell
This article describes the PowerShell cmdlets that are used to manage your .appx and .msix packages.

## MSIX PowerShell cmdlets

| PowerShell cmdlets | Description |
|-------------------|-------------|
| [Add-AppPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) | This cmdlet is used to add a signed app (*.msix, *.appx) package to a device. The Add-AppPackage cmdlet can also be used when adding an MSIX app that has a relationship to another MSIX app such as: External Packages, [Optional Packages](../package/optional-packages.md), and [Related Packages](../package/optional-packages.md). |
| [Remove-AppPackage](/powershell/module/appx/remove-appxpackage?view=win10-ps) | This cmdlet is used to remove a signed app (*.msix, *.appx) package from a device. Once removed, the contents of the folder that the signed app was installed to are removed, as well as any references to the uninstalled application on the machine. |
| [Get-AppPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) | This cmdlet will provide a list of all installed signed app (*.msix, *.appx) packages on the machine. A value can be provided to filter the results. To create a filtered return provide a full or partial string into the **-Name** parameter using * as the wild character. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume?view=win10-ps) | This cmdlet will provide the default volume being used by signed app (*.msix, *.appx) packages on the machine. The default volume is the target for all deployment or install operations on a machine. This volume can not be removed from the list of volumes. |
| [Get-AppPackageManifest](/powershell/module/appx/get-appxpackagemanifest?view=win10-ps) | This cmdlet will return a signed app (*.msix, *.appx) package manifest xml object for the specified signed app full package name. |