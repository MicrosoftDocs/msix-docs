---
title: Package storage volumes
description: Learn how Windows stores staged MSIX packages in package volumes and how to create, select, move, mount, and secure those volumes.
ms.date: 09/30/2026
ms.topic: concept-article
keywords: msix, package volume, appx volume, WindowsApps, package storage, staged package
---

# Package storage volumes

A package volume is a Windows-managed installation store for staged packages. The default package volume is normally `C:\Program Files\WindowsApps`, where each package version has a directory based on its package full name.

Package volumes support side-by-side staging. Windows can write a new package version without modifying the version that an application currently uses. This design reduces the risk of a partially applied update.

Not every package is staged in a package volume. Development-mode registrations, packages with external location, system packages, and layouts registered in place can use other storage models.

## Default and additional volumes

Windows uses the default package volume when a deployment operation doesn't specify another target. You can create package volumes on other local drives, removable media, or supported network storage.

Use the `Windows.Management.Deployment.PackageManager` APIs or Appx PowerShell cmdlets to manage package volumes:

| Operation | Windows Runtime API | PowerShell |
|---|---|---|
| Add | [AddPackageVolumeAsync](/uwp/api/windows.management.deployment.packagemanager.addpackagevolumeasync) | [Add-AppxVolume](/powershell/module/appx/add-appxvolume) |
| Enumerate | [GetPackageVolumesAsync](/uwp/api/windows.management.deployment.packagemanager.getpackagevolumesasync) | [Get-AppxVolume](/powershell/module/appx/get-appxvolume) |
| Get default | [GetDefaultPackageVolume](/uwp/api/windows.management.deployment.packagemanager.getdefaultpackagevolume) | [Get-AppxDefaultVolume](/powershell/module/appx/get-appxdefaultvolume) |
| Set default | [SetDefaultPackageVolume](/uwp/api/windows.management.deployment.packagemanager.setdefaultpackagevolume) | [Set-AppxDefaultVolume](/powershell/module/appx/set-appxdefaultvolume) |
| Set offline | [SetPackageVolumeOfflineAsync](/uwp/api/windows.management.deployment.packagemanager.setpackagevolumeofflineasync) | [Dismount-AppxVolume](/powershell/module/appx/dismount-appxvolume) |
| Set online | [SetPackageVolumeOnlineAsync](/uwp/api/windows.management.deployment.packagemanager.setpackagevolumeonlineasync) | [Mount-AppxVolume](/powershell/module/appx/mount-appxvolume) |
| Remove | [RemovePackageVolumeAsync](/uwp/api/windows.management.deployment.packagemanager.removepackagevolumeasync) | [Remove-AppxVolume](/powershell/module/appx/remove-appxvolume) |

For example, create and select a package volume with PowerShell:

```powershell
$volume = Add-AppxVolume -Path "Q:\WindowsApps"
Set-AppxDefaultVolume -Volume $volume
```

Deployment APIs that accept a target volume can override the default for an individual operation. You can also use [MovePackageToVolumeAsync](/uwp/api/windows.management.deployment.packagemanager.movepackagetovolumeasync) or [Move-AppxPackage](/powershell/module/appx/move-appxpackage) to move supported packages.

## Offline volumes

When the underlying storage becomes unavailable, Windows marks the package volume and its packages offline. For example, removing a USB drive that contains a package volume sets the packages' [PackageOffline](/uwp/api/windows.applicationmodel.packagestatus.packageoffline) status.

Windows identifies a package volume by the underlying storage media rather than only by a drive letter. Reconnecting removable storage under a different drive letter doesn't create a different package-volume identity.

## Security and encryption

Windows owns and protects package-volume contents. Staged package files are immutable and shouldn't be modified directly.

Windows encrypts package files in non-system package volumes to preserve MSIX integrity guarantees if the storage is moved to another computer. This encryption protects staged package files; it doesn't replace full-volume encryption such as BitLocker.

File-system capabilities vary. Features that depend on hard links, including some disk-space optimizations, aren't available on file systems that don't support hard links.

## Related content

- [Understanding how packaged desktop apps run on Windows](../desktop/desktop-to-uwp-behind-the-scenes.md)
- [Managing MSIX with PowerShell](../desktop/powershell-msix-cmdlets.md)
- [PackageVolume class](/uwp/api/windows.management.deployment.packagevolume)
- [MSIX PackageVolume: How Package Storage Works](https://devblogs.microsoft.com/insidemsix/msix-packagevolume/)
