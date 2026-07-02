---
title: Preinstalling packaged apps 
description: This article provides an overview of preinstalled apps 
ms.date: 07/02/2026
ms.topic: how-to
author: andreww-msft
ms.author: jken
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio, dism, preinstall, preinstalling, packaged apps, package full name, pfun
---

# Preinstalling packaged apps
There are multiple tools which can be used to install a packaged app to a device for all users:

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

## Staging permissions (ACLs)

When you stage a packaged app with a provisioning tool such as DISM or [Add-AppxProvisionedPackage](/powershell/module/dism/add-appxprovisionedpackage?preserve-view=true), the tool sets the correct file system permissions (ACLs) on the staged files for you. If you stage packages *manually* — for example, to an external volume, a network share, or a staging directory that remote or virtual desktops mount — you must set these ACLs yourself. Without them, registration can fail, or registration succeeds but the app fails to launch because the packaged process can't read its own files.

> [!NOTE]
> The principals listed below are the identities that the deployment runtime and the packaged app container rely on. Confirm them against your environment and grant only the least privilege required before deploying broadly.

### Local or external staging volume

Grant the following on the staging folder and everything it contains so that any user on the device can register and run the app:

| Principal | SID | Access |
|-----------|-----|--------|
| `SYSTEM` | `S-1-5-18` | Full control |
| `Administrators` | `S-1-5-32-544` | Full control |
| `Users` | `S-1-5-32-545` | Read & execute |
| `ALL APPLICATION PACKAGES` | `S-1-15-2-1` | Read & execute |
| `ALL RESTRICTED APPLICATION PACKAGES` | `S-1-15-2-2` | Read & execute |

The `ALL APPLICATION PACKAGES` and `ALL RESTRICTED APPLICATION PACKAGES` entries are required because a packaged app runs inside an app container with a reduced token. If these identities can't read the staged files, the app fails to launch even when registration reports success.

Apply the permissions with `icacls`, using the SIDs so the command works regardless of display language:

```cmd
icacls "D:\MsixStaging" /grant "*S-1-5-18:(OI)(CI)F" "*S-1-5-32-544:(OI)(CI)F" "*S-1-5-32-545:(OI)(CI)RX" "*S-1-15-2-1:(OI)(CI)RX" "*S-1-15-2-2:(OI)(CI)RX"
```

### Network share or virtual desktop staging directory

When packages are staged to an SMB file share that remote or virtual desktops mount during sign-in — for example, a Windows Virtual Desktop or Azure Virtual Desktop staging directory — each session host reads the staged files as its *computer account*. Grant **Read & execute** to each session host computer object, or, for easier management, to an Active Directory security group that contains those computer accounts, on **both**:

- the **NTFS** permissions of the staged files and folders, and
- the **share** (SMB) permissions of the file share.

```cmd
icacls "\\server\share\MsixImages" /grant "CONTOSO\AVD-SessionHosts$:(OI)(CI)RX"
```

For Azure Files and the Azure role-based access control (RBAC) roles required when you use App Attach with Azure Virtual Desktop, see [App attach in Azure Virtual Desktop](/azure/virtual-desktop/app-attach-overview#file-share) and [Set up App Attach](/azure/virtual-desktop/app-attach-setup).

## DISM
DISM is a command-line tool that can be used to service and prepare Windows images, including those used for Windows Pre-Execution (Win-PE), Recovery Environment (Win-RE), and Windows Setup. Dism can be used to service a Windows image (.wim) or virtual hard disks (.vhd, or .vhdx).

## Provisioning packages
All app provisioning is encapsulated within the DISM tool, and it does both the staging and ARS setup. To do provisioning, the IT Pro needs an app package (.msix, .msixbundle, .appx or .appxbundle) and any dependency packages. 

Beginning with Windows 10 1809, IT Pros can pre-install through provisioning. Provisioned apps will be installed to a central location: %ProgramFiles%\WindowsApps and will immediately be available to registered users. Only users with the MSIX app package registered to their account will have access to the app.

In Windows 10 2004, a provisioned packaged app will reinstall during re-provisioning. Prior versions of Windows 10 would prevent the reinstall of these packaged apps if the user had previously uninstalled the packaged app.

### Force Provisioning
With regular provisioning, if a user removes an app, it cannot be reinstalled with an update. With force provisioning, an IT pro administrator can re-provision an app to be reinstalled for all users. This is triggered by running the **[Add-ProvisionedAppxPackage](/powershell/module/dism/add-appxprovisionedpackage?preserve-view=true)** PowerShell command described below.

## PowerShell
List of relevant PowerShell commands
* **[Get-ProvisionedAppxPackages](/powershell/module/dism/get-appxprovisionedpackage?preserve-view=true)** This will list all of the apps that are pre-installed on the image.
* **[Add-ProvisionedAppxPackage](/powershell/module/dism/add-appxprovisionedpackage?preserve-view=true)** This stages the appx package and configures it for pre-install. All dependencies must be provided as well, which can be found in the SDK or with store-downloaded packages.
* **[Remove-ProvisionedAppxPackage](/powershell/module/dism/remove-appxprovisionedpackage?preserve-view=true)** This can be used to remove a pre-installed app. Note that it does not remove the app if it is already registered for any users - this only strips the auto-registration behavior so it will not be auto-installed for any new users.  If no users have yet installed the app, this command will also remove the staged files.

Using the MSIX PowerShell cmdlets, to preinstall or provision a packaged app on a device you must use the MSIX app's Package Full Name. The Package Full Name is the full name of the package containing the package name, version, architecture, and publisher information. The following is an example of a Package Full Name: `Contoso.ContosoApp_44.20231.1000.0_neutral__8wekyb3d8bbwe`

## Licensing
Licensing only applies when provisioning a Windows Store app. Any other apps can be provisioned without a license. If an app is from the Store a machine-license must also provided when the app is provisioned. At this time, all preinstall Windows Store apps must be free apps and configured to be pre-installable via the Windows Store Partner Center. Once it is configured the pre-installable package and license can be downloaded and then provisioned onto any image.
