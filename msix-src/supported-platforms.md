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

## Winodws 10 Feature Breakdown
MSIX is supported on Windows 10 1809 and later. Here is breakdown of key features that are supported on Windows 10. 

> [!div class="mx-tableFixed"]
| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 20H1
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Native MSIX install and uninstall | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| App Installer File Support| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Identity for Packaged Win32 apps | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| allowElevation Permissions | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Modification Package Support | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| ForceUpdateFromAnyVersion Downgrade |  :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| Package Support Framework (PSF) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## Windows 10 Format support 
Here is a breakdown of supported formats on Windows 10 versions. 

| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 20H1
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| .msix              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .msixbundle| :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:|
| .appx | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| .appxbundle |:heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 

## Microsoft Store 
Here is a breakdown of supported features of Microsoft Store on Windows 10 versions

| Features | Windows 1709 | Windows 1803 | Windows 1809 | Windows 1903 | Windows 1909 | Windows 20H1
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|
| Publishing             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Update Notification| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Update Distribution | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 
| Streaming Install | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| 
| Delta Updates | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :heavy_check_mark: | :heavy_check_mark:| 


## Down level OS compatibility 
For MSIX support on  Windows earlier than Windows 10, version 1709. MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) on GitHub that enables these earlier Windows versions to install MSIX packages. 

Visit the [MSIX Core](msix-core/msixcore.md) articles for more information. 

