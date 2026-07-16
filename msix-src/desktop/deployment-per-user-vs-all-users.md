---
title: Per-user vs all-users MSIX deployment and install locations FAQ
description: Answers to frequently asked questions about MSIX per-user and all-users deployment, staging and registration, install locations and package volumes across multiple drives, and multi-user access and filesystem permissions.
ms.date: 07/16/2026
ms.topic: faq
keywords: windows 10, windows 11, msix, per-user, all-users, provisioning, registration, staging, package volume, install location, multiple drives, multi-user, faq
---

# Per-user vs all-users MSIX deployment and install locations FAQ

When you deploy an MSIX package to a machine that has more than one user, or more than one drive, questions come up about where the app is installed and who can use it. This article answers the most common ones. It brings together the staging and registration model from [Preinstalling packaged apps](deploy-preinstalled-apps.md), the install-location and package-volume behavior described in [Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md), and the volume management cmdlets listed in [Managing MSIX with PowerShell](powershell-msix-cmdlets.md).

## How does installing an MSIX package work: is there a separate copy per user?

Installing an MSIX package on a device happens in two steps:

- Staging places one copy of the package files on the local file system. A package is staged only once per machine, and staging can happen before any user account is signed in.
- Registration makes a staged package available to a specific user. Registration happens on a per-user basis. When a user is registered for the app, Windows creates that user's app data, file type associations, and Start menu entries.

The package payload is not duplicated for each user. The files live in a shared location, and adding the app for another user registers the existing staged copy for that user rather than copying a separate per-user payload.

## What is the difference between per-user and all-users deployment?

There are two ways to make a package available to users on a machine.

| Deployment | What it does | Typical tools |
|---|---|---|
| Per-user | Stages the package if needed and registers it for the current user only. | [`Add-AppxPackage`](/powershell/module/appx/add-appxpackage), double-clicking an installer, or the App Installer. |
| All-users (provisioning) | Stages the package once and registers it for users as they sign in. | [`Add-AppxProvisionedPackage`](/powershell/module/dism/add-appxprovisionedpackage), DISM, or provisioning packages. |

Provisioned apps are staged to the central location `%ProgramFiles%\WindowsApps` and become available to registered users. Only users who have the package registered to their account can access the app. For the full provisioning workflow, including DISM, force provisioning, and licensing, see [Preinstalling packaged apps](deploy-preinstalled-apps.md).

## An app is installed for one user. How do I make it available to another user on the same machine?

Because the payload is already staged, making an installed app available to a second user does not reinstall the app. You register the existing package for that user, or provision it for all users so that it registers automatically at sign-in. In both cases the second user runs the same staged files as the first user.

## Can an MSIX app be installed on, or moved to, a different drive?

Yes. The default install location for new packages is `C:\Program Files\WindowsApps\<package_full_name>`. After deployment, the package files are marked read-only and are locked down by the operating system, and Windows prevents the app from launching if those files are tampered with.

That default location is a package volume. `C:\Program Files\WindowsApps` is the default package volume that Windows ships with, but it is not the only place packages can live. You can create a package volume on any drive and at any path, which is what lets MSIX apps be installed on, or moved to, a drive other than the system drive. Moving an installed app to another drive from Settings > Apps uses this same package-volume mechanism.

You manage package volumes and the packages on them with the [MSIX PowerShell cmdlets](powershell-msix-cmdlets.md):

| Cmdlet | Purpose |
|---|---|
| [`Add-AppxVolume`](/powershell/module/appx/add-appxvolume) | Add a new package volume, for example on another drive. |
| [`Get-AppxVolume`](/powershell/module/appx/get-appxvolume) | List the package volumes known to the machine. |
| [`Mount-AppxVolume`](/powershell/module/appx/mount-appxvolume) / [`Dismount-AppxVolume`](/powershell/module/appx/dismount-appxvolume) | Make the apps on a volume accessible or inaccessible. |
| [`Set-AppxDefaultVolume`](/powershell/module/appx/set-appxdefaultvolume) | Choose which mounted volume is the default target for new deployments. |
| [`Move-AppxPackage`](/powershell/module/appx/move-appxpackage) | Move an installed package to another mounted volume. |

To satisfy apps that expect their files at a fixed path outside `WindowsApps`, Windows 11 lets a package project a directory to another location. See [Create a directory in any location based on packaged app directory](../manage/create-directory.md).

## Can the same app live on one drive for some users and a different drive for other users on the same machine?

No. Package volumes are a machine-level concept, not a per-user one. Mounting a package volume makes the apps deployed to it accessible on the device, and dismounting it removes access to those apps. Because the mount state applies to the whole machine, you do not end up in a state where one user can reach a drive's packages and another user on the same machine cannot.

This means:

- The same package is not installed to different physical drives for different users on one machine. There is a single staged copy, and each user is registered against it.
- If the volume a package lives on is not mounted, that app is inaccessible for everyone on the machine until the volume is mounted again. The package is not silently reinstalled to a different drive for a particular user.

For example, if user A can reach every drive but user B can only reach some, you cannot end up with a package that is available to A but not B because of drive access alone. If the package lives on a drive that is not the target you wanted for user B, user B still runs the app from the drive where it was staged rather than having it reinstalled to a different drive.

## Who manages the filesystem permissions for MSIX packages?

The operating system manages the filesystem permissions for MSIX packages. Package files under `WindowsApps` are read-only and access controlled by Windows, and the containerization model keeps each user's app state separate through file system and registry virtualization. You should not modify package files or their permissions by hand. Doing so breaks the package, and Windows integrity enforcement will block the app from launching and start a repair or reinstall. For how this isolation and integrity model works, see [MSIX containerization overview](../msix-containerization-overview.md) and [Enforce Package Integrity Check](tamper-protection.md).

## Related content

- [Preinstalling packaged apps](deploy-preinstalled-apps.md)
- [Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md)
- [Managing MSIX with PowerShell](powershell-msix-cmdlets.md)
- [MSIX containerization overview](../msix-containerization-overview.md)
- [Shared package container](../manage/shared-package-container.md)
