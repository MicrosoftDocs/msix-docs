---
Description: This guide explains MSIX Shared Package Container
title: MSIX Shared Package Container
ms.date: 03/17/2021
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# MSIX Shared Package Container 
Shared package container allows IT Pros to create a shared runtime container for MSIX packaged application – sharing a merged view of the virtual file system and virtual registry - enabling access to one another’s package root files and state. Begining on Windows 10, Dev Insider Build 21354,  IT Pros will be able to to manage what apps can be in what container is important to the conversion of MSIX from legacy installers. The concept of a shared container is used primarily for customization, sharing pre-requisite software, and supporting addons for converted apps. Please note that this is an enterprise only feature and will require administrative privileges to use.  

Shared Package Container operations are independent of app deployment operations. What this means is that apps do not have to be installed prior to share package container definition being deployed to a device. It also means that not all apps that are defined inside the shared package container need to be installed for the shared package container to run. The apps inside the shared package container will be able to independently update without having to modify the shared package container definition.  

Note that an app will only be allowed to be inside one container. Deploying a shared package container that contains an app that is already part of a shared package container will result in an error.  

## Prerequisite  
To use the feature, enterprises will require an administrator on the device. Additionally, the packages will all need to be .msix packages. To package your installers as MSIX package, visit our [create package from existing installer documentation](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/create-an-msix-overview).  

## Shared Package Container definition
Shared package contianer is defined by a .xml file.  The container definition requires a unique name and a list of packages that belong to that container. Note that the priority of the packages is established from top to bottom of the list. Meaning that the top package will have the highest priority. Priority of the package is used for conflict resolutions among packages that may have the same files. Below is a sample of one.  

```xml
<?xml version="1.0" encoding="utf-8"?> 

<AppSharedPackageContainer Name="ContosoContainer"> 

    <PackageFamily Name="Fabrikam.MainApp_8wekyb3d8bbwe" /> 

    <PackageFamily Name="Contoso.MainApp_8wekyb3d8bbwe" /> 

    <PackageFamily Name="ContosoCustomize_7xekyb3d8ccde" /> 

</AppSharedPackageContainer> 
  
    
```
When you have the container definition .xml, you can use the following Powershell commands to  you can use the following PowerShell commands to deploy, reset, update, and remove a Shared Package Container from the device. Note that all other app deployment commands remain the same (i.e installing packages)

### PowerShell Commands 

#### Deploy a Shared Package Container Definition 
```powershell
Add-AppSharedPackageContainer <path> 
``` 
This command deploys the shared package container definiton for the particular user. Optional parameters include the following: 
|**Parameter** |	**Description**|
|---------|---------|
|RequirePackagePresent |Fails if the user does not have a container-specified package registered. |
|ForceApplicationShutdown |Closes all packages currently running in the Shared Package Container. |
|ProvisionForAllUsers  |Provisions the container for all users. |

#### Remove a shared package container 
```powershell
Remove-AppSharedPackageContainer -Name <name>  
``` 
This command removes the shared package container definiton for the particular user. Optional parameters include the following: 

|**Parameter** |	**Description**|
|---------|---------|
|ForceApplicationShutdown  |Closes all packages in the Shared Package Container.  |
|AllUsers  |Remove matching packages that are deployed to any user. |
|DeprovisionForAllUsers   |Deprovisions the container for all users.  |

#### Get information on a shared package container 
```powershell
Get-AppSharedPackageContainer -Name <name> 
``` 
This command gets information about the shared package container. In particular, it will show what packages are inside the shared package container. Optional parameters include the following: 

|**Parameter** |	**Description**|
|---------|---------|
|AllUsers   |Get matching packages that are either deployed to any user or are provisioned to the machine.   |

#### Reset shared package container 
```powershell
Reset-AppXSharedPackageContainer -Name <name>  
``` 
This command destroys all the application data of the container, including the virtual files and registry keys. 




