---
title: Duplicate MSIX Packaging Tool Settings across multiple devices
description: Learn how to duplicate your MSIX Packaging Tool Settings across multiple devices 
author: dianmsft
ms.author: diahar
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---
# Duplicating MSIX Packaging Tool Settings across multiple devices 
One feature ask we have been receiving from our users is an easy way to duplicate settings for the MSIX Packaging Tool across multiple devices, or even a back up and restore if you need to reimage a device. 
Here are a couple simple steps you can do to easily duplicate the same settings for your users. 
 
## Build out the baseline 
1. On a device with the MSIX Packaging Tool installed, click the settings Icon. 
2. Configure the settings to meet the specific requirements of your environment.   
Note: This generally means tweaking the file and registry exclusions but can include any of the settings.  
3. Once you have configured the settings, make sure you click Save settings, then exit the app.  
 
## Back up the settings configuration
- From the device with the configured baseline, copy this file to a known good location (probably a network server share). 

C:\Users\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

Note: Make sure folks don’t delete or alter this file because it will be your template for all other installations. 
 
## Restore the settings configuration
When a new device is setup with the MSIX Packaging Tool installed, copy the settings.xml from the known good location back to the local device: C:\Users\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 
 
## Considerations 
- Make sure the <DefaultSaveLocation> entry is removed or a valid location for a user.  If the value is not provided or location is invalid, we will default to:  C:\Users\%username%\Desktop 
- Settings files should only be copied to the same version of the packaging tool or later.  Settings may not be honored if the version in the settings.xml is copied to an older version of the tool.  It’s a best practice to use the latest tool anyway since we probably made some fixes to improve the conversion experience.  
