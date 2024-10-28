---
title: Related App Installer file documentation
description: This article provides links to documentation about the App Installer file schema and related APIs provided by the Windows SDK.
ms.date: 2/20/2019
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, API, XML, schema
ms.custom: 19H1
---

# App Installer file APIs

The [PackageManager](/uwp/api/windows.management.deployment.packagemanager) and [Package](/uwp/api/windows.applicationmodel.package) classes in the Windows SDK provide methods you can use to add or modify packages via App Installer files, or to retrieve information about apps with an App Installer association.

|  Method  |  Description | Minimum supported release |
|----------|--------------|-------------------|
|  [PackageManager.AddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.addpackagebyappinstallerfileasync)  | Allows single or multiple app packages to be installed with an .appinstaller file. | Windows 10 Fall Creators Update (version 1709, build 16299)   |
|  [PackageManager.RequestAddPackageByAppInstallerFileAsync](/uwp/api/windows.management.deployment.packagemanager.requestaddpackagebyappinstallerfileasync)  | Allows single or multiple app packages to be installed with an .appinstaller file. | Windows 10 Fall Creators Update (version 1709, build 16299)       |
|  [Package.GetAppInstallerInfo](/uwp/api/windows.applicationmodel.package.getappinstallerinfo)  | Returns the .appinstaller xml file location. This allows app developers to retrieve the .appinstaller xml file location when needed by their app. | Windows 10, version 1809 (build 17763) |
|  [Package.CheckUpdateAvailabilityAsync](/uwp/api/windows.applicationmodel.package.checkupdateavailabilityasync)  | Checks for updates to the main app package listed in the .appinstaller file. It allows the developer to determine if the updates are required due to .appinstaller policy. This method currently only works for applications installed via .appinstaller files. | Windows 10, version 1809 (build 17763) |

## App Installer file schema

For more information about how to manually format an App Installer file, see [App installer file schema reference](/uwp/schemas/appinstallerschema/app-installer-file).
