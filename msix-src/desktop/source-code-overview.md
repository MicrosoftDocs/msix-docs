---
description: Learn how to build an MSIX package from source code in your build environment instead of generating an installer.
title: Building an MSIX package from your code overview
author: Huios
ms.date: 02/03/2020
ms.topic: concept-article
keywords: windows 10, uwp
ms.custom: RS5
---


# Building an MSIX package from your code 

If your desktop application is in active development we recommend building an MSIX package in your build environment instead of generating an installer and running it through the MSIX Packaging Tool. In Visual Studio 2017 version 15.5 and later (including Visual Studio 2019) you can use the Windows Application Packaging Project to generate an MSIX for your application. If you're not developing in Visual Studio there are MSIX command line tools you can integrate into your build system to package your application binaries as MSIX.

If you're developing a UWP application, Visual Studio will default to MSIX as the packaging format for your application.

|Topic| Description |
|:---|:---|
|[What to know before packaging your desktop app](before-packaging-overview.md)| Background on MSIX requirements and packaged desktop app runtime behavior. This is useful to know before building an MSIX package for your desktop application. If you're building a UWP app you can skip this section. | 
|[Packaging your desktop or UWP app in Visual Studio](vs-package-overview.md)| This section discusses how to package your desktop (Windows Forms, WPF, Win32 etc.) or UWP app as an MSIX in Visual Studio.|
|[CI/CD Pipelines for MSIX Builds and Deployments](azure-dev-ops.md)| This section discusses how to automate your build and deployment workflows using CI/CD pipelines in Azure DevOps.|
|[Packaging from the command line](../package/manual-packaging-root.md)| This section discusses how to package your app as an MSIX using command line tools.|
|[Extending your MSIX application](extend-overview.md)| This section discusses how you can to extend your application using extensions and optional packages.|

## Add modern Windows 10 experiences

After you create an MSIX package for your desktop app, you can use UWP APIs, package extensions, and UWP components to light up modern and engaging Windows 10 experiences such as live tiles and notifications.

### Enhance with UWP APIs

Once you've packaged your app, you can light it up with features such as live tiles, and push notifications. Some of these capabilities can significantly improve the engagement level of your application and they cost you very little time to add. Some enhancements require a bit more code.

See [Use UWP APIs in desktop applications](/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### Integrate with package extensions

If your application needs to integrate with the system (For example: establish firewall rules), describe those things in the package manifest of your application and the system will do the rest. For most of these tasks, you won't have to write any code at all. With a bit of XML in the manifest, you can do things like start a process when the user logs on, integrate your application into File Explorer, and add your application a list of print targets that appear in other apps.

See [Integrate your desktop application with package extensions](/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### Extend with UWP components

Some Windows 10 experiences (For example: a touch-enabled UI page) must run inside of an AppContainer. In general, you should first determine whether you can add your experience by [enhancing](/windows/apps/desktop/modernize/desktop-to-uwp-enhance) your existing desktop application with UWP APIs. If you have to use a UWP component, to achieve the experience, then you can add a UWP project to your solution and use app services to communicate between your desktop application and the UWP component.

See [Extend your desktop application with UWP components](/windows/apps/desktop/modernize/desktop-to-uwp-extend).
