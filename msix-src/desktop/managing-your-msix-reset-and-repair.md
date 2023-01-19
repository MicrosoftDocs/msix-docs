---
description: This article provides details regarding the the reset and repair of MSIX apps that have been deployed to a device.
title: Reset or Repair MSIX Apps
ms.date: 11/10/2020
ms.topic: article
keywords: windows 10, deployment, msix, reset, repair
ms.assetid:  
---
  
# Reset or Repair MSIX apps

## Overview

Windows apps (*.msix, *.msixbundle, *.appx, and *.appxbundle) offer functionality known as App Reset, and Repair. Which can be performed by the device user (without elevation) to re-install or refresh the Windows app installation in an attempt to resolve a failure with a Windows app.

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
