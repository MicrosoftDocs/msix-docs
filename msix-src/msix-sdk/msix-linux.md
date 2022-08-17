---
title: How to build MSIX package on Linux 
description: This article gives an overview of how to create an MSIX package on a Linux machine using the MSIX SDK
ms.date: 08/16/2022
ms.topic: article
keywords: windows 10, windows 11, linux, uwp, msix
ms.custom: RS5
---

# How to build MSIX package on Linux
The MSIX SDK project includes cross-platform API support for packing and unpacking .msix/.appx packages. The project includes a shared library (.so file) that enables MSIX packages to be packaged on Linux. This library exports a subset of the functionality contained within appxpackaging.dll on Windows.

To learn more about creating, reading and writing an app package, visit the [Packaging API](/windows/win32/appxpkg/interfaces) page. 

## Build the MSIX package
On a Linux machine use the following commands to build an MSIX package:

```
   ./makelinux [options]
   ./makeaosp [options]
```

## Use the MSIX package

After creating your MSIX package on Linux, you have a few options:

* Distribute your app on the Microsoft Store for business. There is no need to sign it yourself in this case.

* Use this Azure Dev Ops [MSIX Packaging Extension](https://marketplace.visualstudio.com/items?itemName=MSIX.msix-ci-automation-task) to help sign your package in a Windows agent. A sample pipeline can be [found here](https://github.com/microsoft/msix-packaging/blob/master/tools/pipelines-tasks/azure-pipelines/sample-pipeline.yml).

* Generate the MSIX package on Linux and copy it over to a Windows machine to sign it by using [signtool](/windows/msix/package/sign-app-package-using-signtool).
