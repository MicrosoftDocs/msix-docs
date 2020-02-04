---
Description: An overview of topics for creating an MSIX package from source code
title: Building an MSIX package from your code overview
author: Huios (Tanaka Jimha)
ms.date: 02/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Building an MSIX package from your code 

If your desktop application is in active development we recommend building an MSIX package in your build environment instead of generating an installer and running it through the MSIX Packaging Tool. In Visual Studio 2017 version 15.5 and later (including Visual Studio 2019) you can use the Windows Application Packaging Project to generate an MSIX for your application. If you're not developing in Visual Studio there are MSIX command line tools you can integrate into your build system to package your application binaries as MSIX.
If you're developing a UWP application, Visual Studio will default to MSIX as the packaging format for your application.

|Topic| Description |
|:---|:---|
|[What to know before packaging your desktop app](before-packaging-overview.md)| Background on MSIX requirments and packaged Desktop app runtime behaviour. This is useful to know before building an MSIX package for your Desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your desktop or UWP app in Visual Studio](vs-package-overview.md)| This section discusses how to package your desktop (Windows Forms, WPF, Win32 etc.) or UWP app as an MSIX in Visual Studio.|
|[CI/CD Pipelines for MSIX Builds and Deployments](azure-dev-ops.md)| This section discusses how to automate your build and deployment workflows using CI/CD pipelines in Azure DevOps.|
|[Packaging from the command line](../package/manual-packaging-root.md)| This section discusses how to package your app as an MSIX using command line tools.|
|[Extending your MSIX application](extend-overview.md)| This section discusses how you can to extend your application using extensions and optional packages.|

