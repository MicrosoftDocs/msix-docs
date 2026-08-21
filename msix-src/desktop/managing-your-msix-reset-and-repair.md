---
description: This article provides details regarding the the reset and repair of MSIX apps that have been deployed to a device.
title: Reset or Repair MSIX Apps
ms.date: 07/02/2026
ms.topic: article
keywords: windows 10, deployment, msix, reset, repair
ms.assetid:  
---
  
# Reset or Repair MSIX apps

## Overview

Windows apps (*.msix, *.msixbundle, *.appx, and *.appxbundle) offer functionality known as App Reset, and Repair. Which can be performed by the device user (without elevation) to re-install or refresh the Windows app installation in an attempt to resolve a failure with a Windows app.

## Capture app data before you reset or repair

Resetting an app **permanently deletes its data**, and repairing an app can change it. If an app is in a broken state and you want to investigate what caused the failure, copy the app's data *before* you reset or repair it. This lets you diagnose the problem (or share the data with the app publisher) without losing the state that reproduces it.

A packaged app's per-user data is stored under:

```
%LocalAppData%\Packages\<PackageFamilyName>\
```

which expands to `C:\Users\<user>\AppData\Local\Packages\<PackageFamilyName>\`. The most useful subfolders when diagnosing an issue are:

| Subfolder | Contents |
|-----------|----------|
| `LocalState` | Per-user local app data that persists across sessions. |
| `LocalCache` | Non-roaming cached data, including files redirected by the [Virtual File System (VFS)](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes). |
| `RoamingState` | App data intended to roam across the user's devices. |
| `Settings` | The app's virtualized registry settings, stored in `settings.dat`. |

To find the app's package family name, use [Get-AppxPackage](/powershell/module/appx/get-appxpackage):

```PowerShell
Get-AppxPackage -Name "MyEmployees" | Select-Object PackageFamilyName
```

Then copy the folder to a safe location before you reset or repair the app:

```PowerShell
$pfn = (Get-AppxPackage -Name "MyEmployees").PackageFamilyName
Copy-Item -Path "$env:LOCALAPPDATA\Packages\$pfn" -Destination "$env:USERPROFILE\Desktop\MyEmployees-AppData-backup" -Recurse
```

For background on how packaged apps store and virtualize their data, see [Understand how packaged desktop apps run on Windows](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes).

## Repair

You can repair Windows apps that have been installed to an online Windows image. When you use the Windows Settings to repair a Windows app, it will retain the app's data, as it attempts to re-load the application.

### Repairing a Windows app

Repairing a Windows app using the Windows Settings:
    1. Open the Settings app from the Start Menu.
    1. Select the **Apps** menu item.
    1. Select the **Apps & Features** menu item, from the left side navigation.
    1. Select the app that needs to be reset.
    1. Select the **Advanced options** link.
    1. Select the **Repair** button.


## Reset

You can reset Windows apps that have been installed to an online Windows image. When you use the PowerShell cmdlets, or the Windows Settings app to reset a Windows app, it will permanently delete the app's data, and re-install the app fresh. The Windows app will lose preferences, and/or sign-in details, etc.

> [!WARNING]
> Reset **permanently deletes** the app's data and it can't be recovered afterward. If you might need the data to diagnose a failure, [capture the app data](#capture-app-data-before-you-reset-or-repair) before you reset the app.

### Resetting a Windows app

Resetting a Windows app using Windows PowerShell: 
    1. Open an administrative PowerShell window, and type:
        ```PowerShell
        Get-AppxPackage -Name "MyEmployees" | Reset-AppxPackage
        ```

Resetting a Windows App using the Windows Settings:
    1. Open the Settings app from the Start Menu.
    1. Select the **Apps** menu item.
    1. Select the **Apps & Features** menu item, from the left side navigation.
    1. Select the App that needs to be reset.
    1. Select the **Advanced options** link.
    1. Select the **Reset** button.
    1. In the confirmation prompt, select the **Reset** button.
