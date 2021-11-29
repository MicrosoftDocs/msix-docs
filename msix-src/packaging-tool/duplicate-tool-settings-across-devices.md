---
title: Duplicate MSIX Packaging Tool Settings across multiple devices
description: Learn how to duplicate your MSIX Packaging Tool Settings across multiple devices
author: sharlaakers
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# Duplicate MSIX Packaging Tool Settings across multiple devices 

Follow these simple steps to duplicate settings for the MSIX Packaging Tool across multiple devices, or to back up and restore settings if you need to re-image a device. This feature is available starting in version 1.2019.110.0 of the MSIX Packaging Tool. 

## Build the baseline

1. On a device with the MSIX Packaging Tool installed, click the settings Icon.
2. Configure the settings to meet the specific requirements of your environment.
    > [!NOTE]
    > This generally means tweaking the file and registry exclusions but can include any of the settings.
3. After you configure the settings, make sure you click Save settings and then close the tool.  

## Back up the settings configuration

From the device with the configured baseline, copy the following file to a known good location, such as network server share.

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

> [!NOTE]
> Make sure this file isn't deleted or altered, because it will be your template for all other installations.

## Restore the settings configuration

When a new device is setup with the MSIX Packaging Tool installed, copy the settings.xml from the known good location back to the local device: 

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 

## Considerations

* Make sure the \<DefaultSaveLocation\> entry is removed or is a valid location for a user. If the value is not provided or the location is invalid, the tool will default to C:\Users\\%username%\Desktop.

* Settings files should only be copied to the same version of the packaging tool or a later version. Settings may not be honored if the version in the settings.xml is copied to an older version of the tool. We recommend that you always use the latest version of the tool for the latest fixes and for the best packaging experience.  
