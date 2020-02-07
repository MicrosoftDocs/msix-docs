---
Description: Visual Studio makes it easy to generate MSIX packages and is the recommended approach for applications in development.
title: Package your app as an MSIX in Visual Studio 
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Package your app as an MSIX in Visual Studio
Visual studio makes it easy to generate MSIX packages and is the recommended tool for packaging applications in development. Desktop apps (Windows Forms, WPF, Win32 etc.) can use the Windows Application Packaging Project to generate an MSIX package and UWP apps have MSIX generation built into their default project type. To generate an MSIX package for a desktop application you first need to add the Windows Application Packaging Project to your solution, this step is not required for UWP apps.

|Topic| Description |
|:---|:---|
|[Setup your desktop app for packaging in Visual Studio](desktop-to-uwp-packaging-dot-net.md)| This section discusses how to setup your desktop application (Winforms, WPF, Win32 etc.) for packaging using the Windows Application Packaging Project in Visual Studio. | 
|[Packaging a desktop or UWP app in Visual Studio](../package/packaging-uwp-apps.md)| This section discusses how to generate MSIX packages for your desktop or UWP apps in Visual Studio.|
|[Extending your packaged application](extend-overview.md)| This section discusses how to extend your application using extensions and optional packages.|