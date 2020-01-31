---
Description: An overview of topics for creating an MSIX package from source code
title: Building an MSIX package from your code 
author: Huios (tajimha)
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Building an MSIX package from your code 
If your desktop application is in active development we recommend building an MSIX package in your build environment instead of generating an installer and running it through the MSIX Packaging Tool. In Visual Studio 2017 version 15.5 and later (including Visual Studio 2019) you can use the App Packaging Project to generate an MSIX for your application. If you're not developing in Visual Studio we also have command line tools you can integrate into your custom build system to package your app binaries as MSIX.
If you're developing a UWP application, Visual Studio will default to MSIX as the packaging format for your application.

|Topic| Description |
|:---|:---|
|[What to know before packaging your Desktop app](before-packaging-overview.md)| Background on MSIX requirments and packaged Desktop app runtime behaviour. This is useful to know before building an MSIX package for your Desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your Desktop app in Visual Studio](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to package your Desktop app (e.g. Winforms, WPF, Win32) as an MSIX in Visual Studio.|
|[Packaging your UWP app in Visual Studio](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to package your UWP app as an MSIX in Visual Studio.|
|[Packaging from the command line](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to package your app as an MSIX using command line tools.|
|[Extending your MSIX application](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how you can to extend your application using extensions and optional packages.|

