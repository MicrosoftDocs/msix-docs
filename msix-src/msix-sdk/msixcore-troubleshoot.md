---
title: Troubleshooting Tips 
description: This article list out the error codes and issues that customers may face when working with MSIX Core 
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Troubleshooting Tips for MSIX Core 
## Error codes 
Here are some common error messages that you may encounter. 

| Error Code |Description |
|------------|------------|
| 0x8BAD0042 | This typically means that the certificate the app was signed with is not installed. To solve this install the certificate and retry| 
| 0x80070032 | The package is not supported on the device. This can be that the device you are currently on is not one of the target device(s) the package has listed in it's manifest. To resolve this open the package and verify that the device you are on is a target device. This can also occur if the package is targeting a higher version.  | 
|0x8BAD0071 | This error means that you are attempting to install a bundle. MSIX Core currently does not support bundles|

For a full list, visit your [MSIX Core Error Code](https://github.com/microsoft/msix-packaging/blob/master/src/inc/public/MsixErrors.hpp) page. 

## Running MSIX Tracing 
Run the [MSIX Tracing Powershell script](https://github.com/microsoft/msix-packaging/blob/master/preview/MsixCore/Tests/msixtrace.ps1) to identify what is currently happening when installing an MSIX app with MSIX Core. 
