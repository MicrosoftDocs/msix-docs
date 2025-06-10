---
description: This article describes known issues and troubleshooting tips for Sign Tool
title: Known issues and troubleshooting with SignTool
ms.date: 02/05/2020
ms.topic: troubleshooting-known-issue
author: andreww-msft
keywords: windows 10, uwp, msix
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

Another common error is 0x80080057. You may experience the following issues when you try to sign a Portable Executable (PE) file by using the SignTool tool on Windows:

- Failure to sign a PE file that is 4 gigabytes (GB) or larger. When you try to sign, you receive an "invalid parameter (0x80080057)" error message.
- For files that are larger than 4 GB, the generated hash may not be accurate even though SignTool may otherwise successfully sign the file.

   > [!NOTE]
   > This is especially true of .cat files.

This issue occurs for PE files such as .exe, .sys, and so on. This issue occurs because of a ULONG variable in the PE header that specifies the image size. (The image size is 2 GB for down-level operating systems, such as Vista and earlier versions.) This is a design limitation since 1996. The maximum limit for this value is 4 GB for PE files, such as .exe and .sys. Although .cat files are usually signable, the internal hash that's generated may not be accurate.

To work around this issue, make sure that any PE file that you try to sign is less than 4 GB. Because of backward compatibility risks, neither backports nor a permanent fix are currently possible. However, this issue is being investigated.

> [!NOTE]
> This issue isn't specific to SignTool. The design of the PE header is limited to 4 GB for Windows 7 and later Windows versions, regardless of which tool is used.

## Frequently asked questions (FAQ)

Q1: What is the current, official file size limit for a digital signature (and time stamp counter-signature) on Windows?

A1: For PE files such as .exe and .sys, the maximum file size for signing is 4 GB.

Q2: Is there a particular version of Windows, such as Windows Server 2016, that has the most capability to sign large files?

A2: No, the issue affects all versions of Windows.

Q3: Does the 64-bit version of Signtool have better support for this functionality than the 32-bit version does?

A: No, the 64-bit version of SignTool uses the same values as the 32-bit version. Therefore, the issue remains in 64-bit.

Q4: Would customers who are using a 32-bit version of Windows potentially experience issues if they try to use files that were signed by using the 64-bit version of SignTool?

A: No. However, the limitations would remain regardless of which version of SignTool is used.

Q5: Should we be using a different signing tool or method altogether?

A: We currently have no alternative method for digital signing.





