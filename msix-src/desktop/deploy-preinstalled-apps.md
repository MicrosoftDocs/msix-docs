---
title: Preinstalling packaged apps 
description: This article provides an overview of preinstalled apps 
ms.date: 11/30/2020
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio, dism, preinstall, preinstalling, packaged apps, package full name, pfun
---

# Preinstalling packaged apps
There are multiple tools which can be used to install an MSIX packaged app to a device for all users:

- Deployment Image Servicing and Management (DISM)
- Provisioning Packages
- PowerShell

This article will provide an overview of how preinstalled apps work and how provisioning and licenses work with preinstalled apps. 

## Overview
Preinstall of packaged app installations can be broken down into two steps: 
1. Staging
1. Registration

### Staging
Staging a packaged app to a device, is the act of storing a copy of the packaged app to the local file system. A packaged app must only be staged once, and can be performed without any user accounts existing on the device.

The staging of a packaged app can be performed on an offline image (.wim, .vhd, or .vhdx) or an online active operating system. 

### Registration
After a packaged app has been staged, the app can then be registered to users on the device. Registration occurs on a per-user basis, and begins when a user of the device logs on. The operating system will then load the preinstalled packaged app package creating user specific app data, create file type associations, and app tiles in the start menu. This accomplished by the App Rediness Service (ARS) which is aware of all pre-installed apps. 

## DISM
DISM is a command-line tool that can be used to service and prepare Windows images, including those used for Windows Pre-Execution (Win-PE), Recovery Environment (Win-RE), and Windows Setup. Dism can be used to service a Windows image (.wim) or virtual hard disks (.vhd, or .vhdx).

## Provisioning packages
All app provisioning is encapsulated within the DISM tool, and it does both the staging and ARS setup. To do provisioning, the IT Pro needs an app package (.msix, .msixbundle, .appx or .appxbundle) and any dependency packages. 

Beginning with Windows 10 1809, IT Pros can pre-install through provisioning. Provisioned apps will be installed to a central location: %ProgramFiles%\WindowsApps and will immediately be available to registered users. Only users with the MSIX app package registered to their account will have access to the app.

In Windows 10 2004, a provisioned packaged app will reinstall during re-provisioning. Prior versions of Windows 10 would prevent the reinstall of these packaged apps if the user had previously uninstalled the packaged app.

## PowerShell
List of relevant PowerShell commands
* **[Get-ProvisionedAppxPackages](/powershell/module/dism/get-appxprovisionedpackage?view=win10-ps)** This will list all of the apps that are pre-installed on the image.
* **[Add-ProvisionedAppxPackage](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)** This stages the appx package and configures it for pre-install. All dependencies must be provided as well, which can be found in the SDK or with store-downloaded packages.
* **[Remove-ProvisionedAppxPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)** This can be used to remove a pre-installed app. Note that it does not remove the app if it is already registered for any users - this only strips the auto-registration behavior so it will not be auto-installed for any new users.  If no users have yet installed the app, this command will also remove the staged files.

Using the MSIX PowerShell cmdlets, to preinstall or provision an MSIX packaged app on a device you must use the MSIX app's Package Full Name. The Package Full Name is the full name of the package containing the package name, version, architecture, and publisher information. The following is an example of a Package Full Name: `Contoso.ContosoApp_44.20231.1000.0_neutral__8wekyb3d8bbwe`

## Licensing
Licensing only applies when provisioning a Windows Store app. Any other apps can be provisioned without a license. If an app is from the Store a machine-license must also provided when the app is provisioned. At this time, all preinstall Windows Store apps must be free apps and configured to be pre-installable via the Windows Store Partner Center. Once it is configured the pre-installable package and license can be downloaded and then provisioned onto any image.
