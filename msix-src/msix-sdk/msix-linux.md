---
title: How to build MSIX package on Linux 
description: This article gives an overview of how to create an MSIX package on a Linux machine using the MSIX SDK
ms.date: 02/02/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# How to build MSIX package on Linux
The MSIX SDK project includes cross-platform API support for packing and unpacking .msix/.appx packages. The project includes a shared library (.so file) that enables MSIX packages to be packaged on Linux. This library exports a subset of the functionality contained within appxpackaging.dll on Windows. 

To learn more about creating, reading and writing an app package, visit the [Packaging API](https://docs.microsoft.com/windows/win32/appxpkg/interfaces?redirectedfrom=MSDN) page. 

## Build the MSIX package
On a Linux machine use the following commands to build an MSIX package 

```
   ./makelinux [options]
   ./makeaosp [options]
```