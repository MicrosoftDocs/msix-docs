---
Description: This article provides details regarding the options available when updating an MSIX app.
title: Differential updates MSIX app package
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---
  
# Differential updates for MSIX app packages

## Understanding MSIX app package updates
When an MSIX app package are created, a manifest file is generated containing details related to the files included in the MSIX app package. During package creation, a piece of metadata is created and stored in the .msix or .msixbundle package which allows parts of the package to be uniquely identified by the Windows. Later on, during update, Windows can use this metadata file to compare the old package to the new package and determine the things that need to be downloaded to the device. Given this metadata allows parts of the package to be uniquely identified, this means the differential update machinery fully functions from any version of a package to any other version of a package (assuming source package has a lower version than target package).

It all starts at the AppxBlockMap.xml file (the aforementioned metadata). The AppxBlockMap.xml file is an XML document that contains a two dimensional list of information about files in the package. The first dimension lays out high level details on the file (e.g. name and size) and the second dimension provides SHA2-256 hash representations of each 64KB slice of that file (aka the "block").

The first hash represents the first 64KB block of the file and the second hash represents the remaining 35KB - given the file is 101188 bytes.

During an update, if the second block of that file was modified, the hash would also be updated to reflect this fact. The download component understands this and will only pull down the second block and reuse the first unchanged block from the old package.

Furthermore, if an entire file hasn't changed (which is determined by the full set of blocks not changing) then that file can be reused from the existing package - resulting in tremendous savings for Windows 10 users

### Upgrading to newer versions
When a newer version of the MSIX app package is installed, the manifest file is compared and modified file blocks are identified. As the MSIX app package is upgraded to the newer version only the modified files are retrieved decreasing the bandwidth consumption if updated applications reside on a network share or external to an organization.

### Upgrading to older versions
When an older version of the MSIX app package is installed, the manifest file is compared and modifed file blocks are identified. As the MSIX app package is upgraded to the older version only the modified files are retrieved decreasing the bandwidth consumption if updated applications reside on a network share or external to an organization.

## Optimizing upgrade experiences
The delivery or installation of an MSIX app package to a device can be configured to improve the users experience. When an app is deployed the device can be configured to update the app after the user closes the app , or force the application to be closed and update the app forcably.

### PowerShell
Installing an MSIX app package to a device using PowerShell leverages the [add-appxpackage](/powershell-msix-cmdlets.md) cmdlet. This cmdlet contains the following parameters which alter the MSIX app package installation or upgrade user experience.

| Parameter | Description |
|-|-|
| -DeferRegistrationWhenPackagesAreInUse | Indicates that this cmdlet will prevent the MSIX app package from updating while the user currently has the app open. |
| -ForceApplicationShutdown | Indicates that this cmdlet forces all active processes that are associated with the package or its dependencies to shut down |
| -ForceUpdateFromAnyVersion | Indicates that the MSIX app package will force a specific version of a package to be staged/registered, regardless of whether a higher version is already stages/registered. |
| -InstallAllResources | Indicates that the cmdlet forces the deployment of all resource packages specified from a bundle argument. This overrides the resource applicability check of the deployment engine and forces staging of all resource packages. |
| -RetainFilesOnFailure | In the case of a failed deployment, if this switch is set to True, files that have been created on the target machine during the installation process are not removed. |
| -Update | Specifies that the package being added is a dependency package update. A dependency package is removed when the parent app is removed. If not specified, the package will not be removed when the parent app is removed. |

For a full list of parameters available for this cmdlet, please visit the PowerShell article on [add-appxpackage](/powershell/module/appx/add-appxpackage?view=win10-ps).