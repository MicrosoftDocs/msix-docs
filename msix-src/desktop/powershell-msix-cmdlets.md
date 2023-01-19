---
description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing MSIX with PowerShell
ms.date: 12/4/2020
ms.topic: article
keywords: windows 10, deployment, msix, PowerShell, PSH, PS, PoSh, cmdlets
ms.assetid:  
---

# Managing MSIX with PowerShell
This article describes the PowerShell cmdlets that are used to manage your .appx and .msix packages.

## MSIX PowerShell cmdlets
The following PowerShell cmdlets are provided with aliases enabling the use of either "Appx" or "App" prefixed nouns (Example: `Add-AppxPackage` can also be used as `Add-AppPackage`).

| PowerShell cmdlets | Description |
|-------------------|-------------|
| [Add-AppxPackage](/powershell/module/appx/add-appxpackage) | This cmdlet is used to add a signed app (*.msix, *.appx) package to a device. The Add-AppPackage cmdlet can also be used when adding an MSIX app that has a relationship to another MSIX app such as: External Packages, [Optional Packages](../package/optional-packages.md), and [Related Packages](../package/optional-packages.md). |
| [Remove-AppxPackage](/powershell/module/appx/remove-appxpackage) | This cmdlet is used to remove a signed app (*.msix, *.appx) package from a device. Once removed, the contents of the folder that the signed app was installed to are removed, as well as any references to the uninstalled application on the machine. |
| [Get-AppxPackage](/powershell/module/appx/get-appxpackage) | This cmdlet will provide a list of all installed signed app (*.msix, *.appx) packages on the machine. A value can be provided to filter the results. To create a filtered return provide a full or partial string into the **-Name** parameter using * as the wild character. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | This cmdlet will provide the default volume being used by signed app (*.msix, *.appx) packages on the machine. The default volume is the target for all deployment or install operations on a machine. This volume can not be removed from the list of volumes. |
| [Get-AppxPackageManifest](/powershell/module/appx/get-appxpackagemanifest) | This cmdlet will return a signed app (*.msix, *.appx) package manifest xml object for the specified signed app full package name. |
| [Reset-AppxPackage](/powershell/module/appx/reset-appxpackage) | This cmdlet will reset the installed app to its original settings. |
| [Get-AppxVolume](/powershell/module/appx/get-appxvolume) | This cmdlet will return a list of AppxVolume objects that are known to the computer. |
| [Add-AppxVolume](/powershell/module/appx/add-appxvolume) | This cmdlet will add a new AppxVolume for the Package Manager to advertise. |
| [Remove-AppxVolume](/powershell/module/appx/remove-appxvolume) | This cmdlet will remove an existing AppxVolume from the device. |
| [Mount-AppxVolume](/powershell/module/appx/mount-appxvolume) | This cmdlet will mount an AppxVolume, allowing all apps that are deployed to the target to become accessible. |
| [Dismount-AppxVolume](/powershell/module/appx/dismount-appxvolume) | Ths cmdlet will dismount an AppxVolume, removing access to apps that are deployed to the target. |
| [Move-AppxPackage](/powershell/module/appx/move-appxpackage) | This cmdlet will move a Windows app packge from its current location to another mounted AppxVolume. |
| [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) | This cmdlet will get the default AppxVolume used as the target for all deployment operations to the devive. |
| [Set-AppxDefaultVolume](/powershell/module/appx/set-appxdefaultvolume) | This cmdlet will set another mounted AppxVolume as the new default target for all deployment operations to the device. |
| [Invoke-CommandInDesktopPackage](/powershell/module/appx/invoke-commandindesktoppackage) | This cmdlet enables the ability to execute commands into the Windows app package bubble. |
