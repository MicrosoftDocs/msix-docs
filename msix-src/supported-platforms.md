---
title: Supported Platform 
description: This article describes supported platform for MSIX. 
author: dianmsft
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# MSIX Supported Platform Overview 
This article describes key features of MSIX and what version of Windows is supported. 

## Windows Feature Breakdown
MSIX is supported on Windows 10 1709 and later. Here is the breakdown of key features on Windows 10. For versions of Windows earlier than Windows 10 version 1709, use [MSIX Core](msix-core/msixcore.md) to install MSIX packages. 

> [!div class="mx-tableFixed"]
| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 2004| Windows Server 2019 (LTSC) | Windows Enterprise 2019 LTSC| 
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Native MSIX install and uninstall | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| :x: | :x: |
| [App Installer File Support](app-installer/installing-windows10-apps-web.md)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Identity for Packaged Win32 apps | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| allowElevation Permissions | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Modification Package Support | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| ForceUpdateFromAnyVersion Downgrade |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Package Support Framework (PSF) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:|  
| Windows Services | :x: | :x: | :x: | :x:| :x: | :heavy_check_mark:| 
| Differ registration flag |  :x: | :x: | :x: | :x:| :heavy_check_mark: | :heavy_check_mark:| 
| Force provisioning | 

flag to differ registration - vb 
force reprovision - ?? 

### Auto elevation
Users running an MSIX package on Windows 10 version 1809 and later can use elevated privileges just by clicking on the app tile. The platform detects that elevation is needed and automatically provides it when the user has the necessary credentials.

On Windows 10 versions 1709 and 1803, users can still run apps with elevated privileges. However, they need to explicitly select to run the app as an administrator. One way to do that is by right-clicking on the app's tile and selecting the **Run as administrator** option.

## Windows 10 Format support 
Here is a breakdown of supported formats on Windows 10 versions. 

| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## Microsoft Store 
Here is a breakdown of supported features of Microsoft Store on Windows 10 versions

| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 2004
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Publishing             | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Update Notification| :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Streaming Install | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| Delta Updates | :x: | :x: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

> [!NOTE]
> .appx or .appxbundle will work for all versions of Windows listed above. The table only reflects .msix or .msixbundle behavior. 

### Microsoft Store submission 
The minimum supported OS version of an MSIX package is listed in the manifest file of the package as `MinVersion` in the `TargetDeviceFamily` element. For example an MSIX package may list `MinVersion="10.0.17701.0"` as the minimum supported version, which means that the MSIX package can run on this and later versions of the OS.

On Windows 10 versions 1709, 1803, and 1809, we support the mainstream enterprise deployment scenarios. These include installation through SCCM, Microsoft Intune, PowerShell or double-click installation.

Currently, MSIX installation through the Microsoft Store and Microsoft Store for Business require Windows 10 version 1803.

## Down level OS compatibility 
For MSIX support on  Windows earlier than Windows 10, version 1709. MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) on GitHub that enables these earlier Windows versions to install MSIX packages. 

Visit the [MSIX Core](msix-core/msixcore.md) articles for more information. 

