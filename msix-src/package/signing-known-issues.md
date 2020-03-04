---
description: This article describes known issues and troubleshooting tips for Sign Tool
title: Known issues and troubleshooting with SignTool
ms.date: 02/05/2020
ms.topic: article
author: dianmsft
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# Known issues and troubleshooting for SignTool
The most common types of errors when using **SignTool** are internal and typically look something like this:

```syntax
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

If the error code starts with 0x8008, such as 0x80080206 (APPX_E_CORRUPT_CONTENT), the package being signed is invalid. If you get this type of error you must rebuild the package and run **SignTool** again.

**SignTool** has a debug option available to show certificate errors and filtering. To use the debugging feature, place the `/debug` option directly after `sign`, followed by the full **SignTool** command.

```syntax
SignTool sign /debug [options]
``` 

A more common error is 0x8007000B. For this type of error, you can find more information in the event log.
 
To find more information in the event log:
- Run Eventvwr.msc
- Open the event log: Event Viewer (Local) -> Applications and Services Logs -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- Find the most recent error event

The internal error 0x8007000B usually corresponds to one of these values:

| **Event ID** | **Example event string** | **Suggestion** |
|--------------|--------------------------|----------------|
| 150          | error 0x8007000B: The app manifest publisher name (CN=Contoso) must match the subject name of the signing certificate (CN=Contoso, C=US). | The app manifest publisher name must exactly match the subject name of the signing.               |
| 151          | error 0x8007000B: The signature hash method specified (SHA512) must match the hash method used in the app package block map (SHA256).     | The hashAlgorithm specified in the /fd parameter is incorrect. Rerun **SignTool** using hashAlgorithm that matches the app package block map (used to create the app package)  |
| 152          | error 0x8007000B: The app package contents must validate against its block map.                                                           | The app package is corrupt and needs to be rebuilt to generate a new block map. For more about creating an app package, see [Create an app package with the MakeAppx.exe tool](create-app-package-with-makeappx-tool.md) |