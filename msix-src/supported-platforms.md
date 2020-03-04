---
title: Supported platforms 
description: This article describes supported platform for MSIX. 
author: dianmsft
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Supported platforms

MSIX is currently supported on the following versions of Windows:

* Windows 10, version 1709, and later.
* Windows Server 2019 LTSC and later.
* Windows Enterprise 2019 LTSC and later.

This article describes how key features of MSIX are supported in these versions of Windows.

> [!NOTE]
> Windows Server 2019 LTSC and Windows Enterprise 2019 LTSC requires the **App Installer** app to support double click install or install directly from a website for .msix, .msixbundle, .appx or .appxbundle. Without the app, packages can be installed via PowerShell, API, or use a supported systems management product. For more considerations about Windows Server 2019 LTSC, see [this article](msix-server-2019.md).

> [!NOTE]
> For versions of Windows earlier than Windows 10 version 1709, use [MSIX Core](msix-core/msixcore.md) to install MSIX packages.

## MSIX feature support

The following table shows which MSIX features and scenarios are supported in different versions of Windows.

> [!div class="mx-tableFixed"]
| Features | 1709 | 1803 | 1809 | 1903 | 1909 | 2004| Windows Server 2019 LTSC | Windows Enterprise 2019 LTSC|
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Native MSIX install and uninstall | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark: |
| [App Installer File Support](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Identity for packaged desktop apps | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Allow elevation | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Modification packages | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Force update from any version downgrade |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Package Support Framework (PSF) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  :heavy_check_mark: | :heavy_check_mark:|  
| Windows services | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x:| :x: | 
| Defer registration flag |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |
| Force provisioning |  :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| :x: | :x: |

## Package format support

The following table shows which package formats are supported in different versions of Windows 10.

| Package format | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## Microsoft Store

The following table shows which Microsoft Store features are supported in different versions of Windows 10.

| Features | 1709 | 1803 | 1809 | 1903 | 1909 | 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Publishing             | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Update Notification| :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Streaming Install | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| Delta Updates | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

> [!NOTE]
> .appx or .appxbundle will work for all versions of Windows 10 listed above. The table only reflects .msix or .msixbundle behavior.

### Microsoft Store submissions

The minimum supported OS version of an MSIX package is listed in the manifest file of the package as `MinVersion` in the `TargetDeviceFamily` element. For example, an MSIX package may list `MinVersion="10.0.17701.0"` as the minimum supported version, which means that the MSIX package can run on this and later versions of the OS.

On Windows 10 versions 1709, 1803, and 1809, we support the mainstream enterprise deployment scenarios. These include installation through Microsoft Endpoint Configuration Manager, Microsoft Intune, PowerShell or double-click installation.

Currently, MSIX installation through the Microsoft Store and Microsoft Store for Business requires Windows 10, version 1809 and later.
