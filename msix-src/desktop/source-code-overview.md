---
Description: This guide provides a list of third-party products and installers to package desktop applications.
title: Package a desktop app using third-party installers
author: dianmsft
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Building an MSIX from your code 
If your desktop application is in active development we recommend building an MSIX package in your build environment instead of generating an installer and running it through the MSIX Packaging Tool. In Visual Studio 2017 version 15.4 and later (including Visual Studio 2019) you can use the App Packaging Project to generate an MSIX for your application. If you're not developing in Visual Studio we also have command line tools you can integrate into your custom build system to package your app binaries as MSIX.
If you're developing a UWP application, Visual Studio will default to MSIX as the packaging format for your application.

|Topic| Description |
|:---|:---|
|[What to know before packaging your Desktop app](sign-app-package-using-signtool.md#prerequisites)| Background on MSIX requirments and packaged Desktop app runtime behaviour. This is useful to know before building an MSIX package for your Desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your Desktop app in Visual Studio](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to package your Desktop app (e.g. Winforms, WPF, Win32) as an MSIX in Visual Studio.|
|[Packaging your UWP app in Visual Studio](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to package your UWP app as an MSIX in Visual Studio.|
|[Extending your MSIX application](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how you can to extend your application using extensions and optional packages.|

