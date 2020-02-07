---
title: deploying preinstalled apps 
description: This article provides an overview of preinstalled apps 
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio
ms.localizationpriority: medium
---

# Deploying preinstalled apps 
This article will provide an overview of how preinstalled apps work and how provisioning and licenses work with preinstalled apps. 

## How preinstalled apps work 

App installation can be broken down into two steps: 
1. Staging 
2. Registration 

When the platform stage an app, the platform place the all the files in the app package into the file system and set appropriate ACLs. Once staged, the app can then be registered for the current user. Registration is where the platform do all the setup to make the app available to the user and integrated with the operating system, such as creating user-specific app data locations, enabling  discovery of app extension points like file type associations, and creating the app tiles. There are many more interesting things that happen but the most important takeaway is that registration absolutely must happen per-user, while staging the app only needs to be done once and can be done without any users. The platform need only stage an app once for any number of users, but each user must be logged in to register the app.

In order to preinstall an app for a user, the platform still need to do these same two steps of stage and register. Staging is easy enough; it can be done anytime and even done offline into the Windows image long before the PC is even manufactured. But registration requires a user to be logged in, and for a new PC or new user account that can't happen until the user signs in for the first time. To complete the pre-installation the platform must register the app on behalf of the user after the user signs in. The platform accomplish this using a system service which knows about all the pre-installed apps that need to be registered and then automatically triggers the registration when the user first signs in. The platform call this service the App Readiness Service, or ARS.

In other words, preinstalled apps work by staging the app package onto disk and then configuring it for automatic registration for each user the next time that user signs in.

## Provisioning apps 
All app provisioning is encapsulated within the DISM tool, and it does both the staging and ARS setup. To do provisioning, the IT Pro needs an app package (.msix, .msixbundle, .appx or .appxbundle) and any dependency packages. 

List of relevant PowerShell commands
* **/Get-ProvisionedAppxPackages** This will list all of the apps that are pre-installed on the image.
* **/Add-ProvisionedAppxPackage** This stages the appx package and configures it for pre-install. All dependencies must be provided as well, which can be found in the SDK or with store-downloaded packages.
* **/Remove-ProvisionedAppxPackage** This can be used to remove a pre-installed app. Note that it does not remove the app if it is already registered for any users - this only strips the auto-registration behavior so it will not be auto-installed for any new users.  If no users have yet installed the app, this command will also remove the staged files.

## Licensing
Licensing only applies when provisioning a Windows Store app. Any other apps can be provisioned without a license. If an app is from the Store a machine-license must also provided when the app is provisioned. At this time, all preinstall Windows Store apps must be free apps and configured to be pre-installable via the Windows Store Partner Center. Once it is configured the pre-installable package and license can be downloaded and then provisioned onto any image.

