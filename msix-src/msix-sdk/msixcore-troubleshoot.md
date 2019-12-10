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
|0x8BAD0071 | This error means that you are attempting to install a bundle. MSIX Core currently does not support bundles.|

The following errors occur when there is an issue with the package format. 

| Error Code |Description |
|------------|:------------|
| 0x8BAD0031 | MissingAppxSignatureP7X|
| 0x8BAD0032 | MissingContentTypesXML|
| 0x8BAD0033 | MissingAppxBlockMapXML|
| 0x8BAD0034 | MissingAppxManifestXML|
| 0x8BAD0035 | DuplicateFootprintFile |
| 0x8BAD0036 | UnknownFileNameEncoding |
| 0x8BAD0037 | DuplicateFile | 

The following errors are related to file issues
 | Error Code |Description |
|------------|:------------|
| 0x8BAD0001 | FileOpen|
| 0x8BAD0002 | FileSeek|
| 0x8BAD0003 | FileRead|
| 0x8BAD0003 | FileWrite|
| 0x8BAD0004 | FileCreateDirectory  |
| 0x8BAD0005 | FileSeekOutOfRange  |

The following errors occur when there is an issue with the certificate the package was signed with. 

| Error Code |Description |
|------------|:------------|
| 0x8BAD0041 | SignatureInvalid| 
| 0x8BAD0042 | CertNotTrusted|
| 0x8BAD0043 | PublisherMismatch|

Other issues you may encounter

| Error Code |Description |
|------------|:------------|
| 0x8BAD0011 | ZipCentralDirectoryHeader|
| 0x8BAD0012 | ZipLocalFileHeader|
| 0x8BAD0013 | Zip64EOCDRecord|
| 0x8BAD0014 | Zip64EOCDLocator |
| 0x8BAD0015 | ZipEOCDRecord |
| 0x8BAD0016 | ZipHiddenData |
| 0x8BAD0017 | ZipBadExtendedData | 

| Error Code |Description |
|------------|:------------|
| 0x8BAD0051 | BlockMapSemanticError|
| 0x8BAD0052 | BlockMapInvalidData|

| Error Code |Description |
|------------|:------------|
| 0x8BAD0061 | AppxManifestSemanticError |
| 0x8BAD0082 | DeflateInitialize |
| 0x8BAD0081 | DeflateWrite |
| 0x8BAD0083 | DeflateRead  |

| Error Code |Description |
|------------|:------------|
| 0x8BAD1001 | XmlWarning  |
| 0x8BAD1002 | XmlError|
| 0x8BAD1003 | XmlFatal |
| 0x8BAD1004 | XmlInvalidData |

For a full list, visit your [MSIX Core Error Code](https://github.com/microsoft/msix-packaging/blob/master/src/inc/public/MsixErrors.hpp) page. 

## Running MSIX Tracing 
Run the [MSIX Tracing Powershell script](https://github.com/microsoft/msix-packaging/blob/master/preview/MsixCore/Tests/msixtrace.ps1) to identify what is currently happening when installing an MSIX app with MSIX Core. 
