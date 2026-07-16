---
title: Per-user vs all-users MSIX deployment and install locations
description: Learn how MSIX stages and registers packages, the difference between per-user and all-users deployment, how packages use install locations and package volumes across multiple drives, and how multi-user access and filesystem permissions behave.
ms.date: 07/16/2026
ms.topic: concept-article
keywords: windows 10, windows 11, msix, per-user, all-users, provisioning, registration, staging, package volume, install location, multiple drives, multi-user
---

# Per-user vs all-users MSIX deployment and install locations

When you deploy an MSIX package to a machine that has more than one user, or more than one drive, two questions come up often:

- If an app is installed for one user, how do you make it available to another user on the same machine?
- Can an MSIX app be installed on, or moved to, a different drive, and how does that interact with multiple users and their access permissions?

This article explains the model MSIX uses so you can reason about these scenarios. It brings together the staging and registration model from [Preinstalling packaged apps](deploy-preinstalled-apps.md), the install-location and package-volume behavior described in [Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md), and the volume management cmdlets listed in [Managing MSIX with PowerShell](powershell-msix-cmdlets.md).

## Staging and registration: one copy on disk, per user access

Installing an MSIX package on a device happens in two steps:

- Staging places one copy of the package files on the local file system. A package is staged only once per machine, and staging can happen before any user account is signed in.
- Registration makes a staged package available to a specific user. Registration happens on a per-user basis. When a user is registered for the app, Windows creates that user's app data, file type associations, and Start menu entries.

The important consequence is that the package payload is not duplicated for each user. The files live in a shared location, and adding the app for another user registers the existing staged copy for that user rather than copying a separate per-user payload.

## Per-user vs all-users deployment

There are two ways to make a package available to users on a machine.

| Deployment | What it does | Typical tools |
|---|---|---|
| Per-user | Stages the package if needed and registers it for the current user only. | [`Add-AppxPackage`](/powershell/module/appx/add-appxpackage), double-clicking an installer, or the App Installer. |
| All-users (provisioning) | Stages the package once and registers it for users as they sign in. | [`Add-AppxProvisionedPackage`](/powershell/module/dism/add-appxprovisionedpackage), DISM, or provisioning packages. |

Provisioned apps are staged to the central location `%ProgramFiles%\WindowsApps` and become available to registered users. Only users who have the package registered to their account can access the app. For the full provisioning workflow, including DISM, force provisioning, and licensing, see [Preinstalling packaged apps](deploy-preinstalled-apps.md).

### Making an app available to another user

Because the payload is already staged, making an installed app available to a second user does not reinstall the app. You register the existing package for that user, or provision it for all users so that it registers automatically at sign-in. In both cases the second user runs the same staged files as the first user.

## Install locations and package volumes

The default install location for new packages is `C:\Program Files\WindowsApps\<package_full_name>`. After deployment, the package files are marked read-only and are locked down by the operating system, and Windows prevents the app from launching if those files are tampered with.

That default location is a package volume. `C:\Program Files\WindowsApps` is the default package volume that Windows ships with, but it is not the only place packages can live. You can create a package volume on any drive and at any path, which is what lets MSIX apps be installed on, or moved to, a drive other than the system drive. Moving an installed app to another drive from Settings > Apps uses this same package-volume mechanism.

You manage package volumes and the packages on them with the [MSIX PowerShell cmdlets](powershell-msix-cmdlets.md):

| Cmdlet | Purpose |
|---|---|
| [`Add-AppxVolume`](/powershell/module/appx/add-appxvolume) | Add a new package volume, for example on another drive. |
| [`Get-AppxVolume`](/powershell/module/appx/get-appxvolume) | List the package volumes known to the machine. |
| [`Mount-AppxVolume`](/powershell/module/appx/mount-appxvolume) / [`Dismount-AppxVolume`](/powershell/module/appx/dismount-appxvolume) | Make the apps on a volume accessible or inaccessible. |
| [`Set-AppxDefaultVolume`](/powershell/module/appx/set-appxdefaultvolume) | Choose which mounted volume is the default target for new deployments. |
| [`Move-AppxPackage`](/powershell/module/appx/move-appxpackage) | Move an installed package to another mounted volume. |

> [!NOTE]
> To satisfy apps that expect their files at a fixed path outside `WindowsApps`, Windows 11 lets a package project a directory to another location. See [Create a directory in any location based on packaged app directory](../manage/create-directory.md).

## Multi-user and multi-drive behavior

Package volumes are a machine-level concept, not a per-user one. Mounting a package volume makes the apps deployed to it accessible on the device, and dismounting it removes access to those apps. Because the mount state applies to the whole machine, you do not end up in a state where one user can reach a drive's packages and another user on the same machine cannot.

This means:

- The same package is not installed to different physical drives for different users on one machine. There is a single staged copy, and each user is registered against it.
- If the volume a package lives on is not mounted, that app is inaccessible for everyone on the machine until the volume is mounted again. The package is not silently reinstalled to a different drive for a particular user.

## Filesystem permissions

The operating system manages the filesystem permissions for MSIX packages. Package files under `WindowsApps` are read-only and access controlled by Windows, and the containerization model keeps each user's app state separate through file system and registry virtualization. You should not modify package files or their permissions by hand. Doing so breaks the package, and Windows integrity enforcement will block the app from launching and start a repair or reinstall. For how this isolation and integrity model works, see [MSIX containerization overview](../msix-containerization-overview.md) and [Enforce Package Integrity Check](tamper-protection.md).

## Related content

- [Preinstalling packaged apps](deploy-preinstalled-apps.md)
- [Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md)
- [Managing MSIX with PowerShell](powershell-msix-cmdlets.md)
- [MSIX containerization overview](../msix-containerization-overview.md)
- [Shared package container](../manage/shared-package-container.md)
