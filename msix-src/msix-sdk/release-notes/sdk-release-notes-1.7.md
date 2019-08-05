---
title: MSIX SDK Release 1.7 release notes
description: Release notes for MSIX SDK release 1.7
ms.date: 02/06/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX SDK 1.7 update

With the SDK release (1.7), we heard the feedback from our partners and added more APIs to provide developers with more options and flexibility in handling MSIX packages. 

### Makemsix 
When typing in ``` makemsix pack -?``` or ```makemsix.exe pack -d <directory> -p <package> [options]```.  in command line, it will display the same information as makeappx commands.

With this release you can now pack your pacakge, you can now using the command ``` makemsix pack``` or use the IAppxPackageWriter interfaces

### Update to msix.dll 
Added the following interfaces to msix.dll 
- Implement the IAppxManifestReader4 interface
- IAppxPackageWriter
- IAppxPackageWriter3 
- IAppxManifestOptionalPackageInfo

### UTF8 API Variants

In this SDK release, we add about 2 new UTF8 API variants for existing API calls. With the inclusion of these new APIs, developers can choose to use the Utf8 variant for string manipulation according to their environment/platform. As with AppxPackaging APIs, the caller is responsible for deallocating the memory used by LPSTR* out parameters.

The following are the new UTF8 interfaces:
- IAppxPackageWriterUtf8
- IAppxPackageWriter3Utf8
- IAppxManifestOptionalPackageInfoUtf8

### Updates to test infrastructure to use Catch2
Before, we had three different implementations to perform our tests:
1. Powershell script, for windows
2. Shell script for Linux and macOS
3. Common shared library used for Android and iOS apps

The change removes the overhead of adding a test in three times by creating a single implementation by using Catch2.

You can get the latest SDK on GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.6" data-linktype="external">MSIX SDK on GitHub</a></p></div>
