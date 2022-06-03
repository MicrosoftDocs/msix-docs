---
title: App package updates
description: Describes how MSIX packages are optimized to ensure that only the essential changed bits of the app are downloaded to update an existing Windows app.
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx, pfan, package family name
ms.custom: "RS5, seodec18"
---

# App package updates

Updating modern Windows app packages is optimized to ensure that only the essential changed bits of the app are downloaded to update an existing Windows app.

## Metadata in the AppxBlockMap.xml file

At a high level, during package creation, a piece of metadata is created and stored in the app package file (.appx or .msix) which allows parts of the package to be uniquely identified by Windows. When updating an app package, Windows uses the metadata file to compare the old package to the new package and determine what needs to be downloaded to the device.

Since the metadata allows parts of the package to be uniquely identified, this means the differential update machinery fully functions from any version of a package to any other version of a package (assuming the source package has a lower version than the target package). 

The metadata is contained in the AppxBlockMap.xml file (the aforementioned metadata). The AppxBlockMap.xml file is an XML file that contains a two dimensional list of information about files in the package. The first dimension lays out high level details on the file (e.g., name and size) and the second dimension provides SHA2-256 hash representations of each 64KB slice of that file (the "block").

Here's a sample of an AppxBlockMap.xml file.

```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<blockmap hashmethod="http://www.w3.org/2001/04/xmlenc#sha256" 
          xmlns="http://schemas.microsoft.com/appx/2010/blockmap">
  <file lfhsize="66" size="101188" name="asset1.jpg">
    <block hash="2bidNE0JyaO+FjaTpRe0g8HzUCblUf/cfBcTXiZR74c="/>
    <block hash="+jeFwKrGk5gw9wSICWsWRtEQXwcLC7af4EWS7DgrAkY="/>
  </file>
  <file lfhsize="61" size="108823" name="asset2.jpg">
    <block hash="u0+5S0GOzwyAfYx54tKycZyHRBYm2ybvq27dkIKqDsQ="/>
    <block hash="F9h0FRMetL6BNCszAYB0bgyx2KWN+dO1bls4Q9m267c="/>
  </file>
  ...
</blockmap>
```

The first file (asset1.jpg) has two block hashes. The first hash represents the first 64KB block of the file and the second hash represents the remaining 35KB - given the file is 101188 bytes.

During an update, if the second block of that file is modified, the hash is also updated to reflect that. The download component pulls down the second block and reuses the first unchanged block from the old package.

On a larger scale, if an entire file does not change (determined by a full set of blocks not changing), that file can be reused from the existing package, saving time and resources.

## App update constraints

#### Updates are performed within the same package family
The package family name is comprised of the Package Name and Publisher. To be able to update, the new package metadata will need to be the same as the previously installed package. The following is an example of a package family name: `Contoso.ContosoApp_8wekyb3d8bbwe`.

#### App updates must increment to a higher version
In general, app updates require the version of the new package to be higher than the current one. The app update process will not allow packages with lower versions to be installed by default. Starting in Windows 10 version 1809, you can use ForceUpdateToAnyVersion to allow lower version packages to be installed when an override switch is provided as part of the update arguments. It is currently available in PowerShell using the [ForceUpdateFromAnyVersion](/powershell/module/appx/add-appxpackage&preserve-view=true) option, via [PackageManager API](/uwp/api/windows.management.deployment.deploymentoptions), [EnterpriseModernAppManagement CSP](/windows/client-management/mdm/enterprisemodernappmanagement-csp) and in the [AppInstaller file](./app-installer/update-settings.md).  

> [!NOTE]
> If you use ForceUpdateToAnyVersion on an app from the Windows Store, Windows Update will automatically update the to the latest applicable version.

#### App update package can have a different architecture
The update package to the currently installed app package can be of a different architecture as long as the new architecture is supported on the OS where it is being deployed to. 
For example: If you have x86 version of MyFavApp(v1.0.0.0) installed on a x64 Windows 10 device and the update package(v2.0.0.0) is x64 version: MyFavApp(1.0.0.0) will be updated to MyFavApp(v2.0.0.0) successfully. 

#### Packages can update from an MSIX to an MSIXbundle
An update package can go from MSIX package to an MSIXbundle package but not vice-versa. When an MSIXbundle is installed, the package update will need to remain a bundle. 

## Optimize differential update technology
    
There are a few ways to ensure that the differential update technology is optimized to the max.

- Keep files in the package small - doing this will ensure that if a change is needed that would impact the full file, the update would still be small.
- Changes to files should be additive if possible - additive changes will ensure that end-user devices only download those changed blocks.
- Changes to files should be contained to 64KB blocks if possible - if your app does have large files and requires changes to the middle of a file, containing changes to a set of blocks will help significantly.
