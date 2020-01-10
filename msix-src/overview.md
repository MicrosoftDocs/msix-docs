---
title: What is MSIX?
description: This article introduces the basics of the MSIX packaging format, a modern packaging experience to all Windows apps.
ms.date: 12/02/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# What is MSIX?

MSIX is a Windows app package format that provides a modern packaging experience to all Windows apps. The MSIX package format preserves the functionality of existing app packages and/or install files in addition to enabling new, modern packaging and deployment features to Win32, WPF, and Windows Forms apps.

> [!TIP]
> Visit the [MSIX Tech Community](https://aka.ms/msixcommunity) page for discussions and latest information.

Here are some brief highlights of MSIX:

* **Package existing Windows apps.** Use the [MSIX Packaging Tool](packaging-tool/mpt-overview.md) to create an MSIX package for any Windows app, old or new. The MSIX packaging tool streamlines the packaging experience, offering an interactive user interface or command line to convert and package Windows apps.
* **Install MSIX app packages.** Use [App Installer](app-installer/app-installer-root.md) to install or update any MSIX app package that is locally available or on any content distribution network.
* **Apply run time fixes to packaged apps.** The [Package Support Framework](psf/package-support-framework-overview.md) is an open source kit that helps you apply fixes to your existing desktop app when you don't have access to the source code, so that it can run in an MSIX container.
* **Use MSIX anywhere.** With the open source [MSIX SDK](msix-sdk/sdk-overview.md), MSIX packages are more versatile, and platform independent. The SDK provides all of the APIs needed to verify, validate, and unpack an app package on any platform, including Windows 10 and non-Windows 10 platforms.

This video introduces the key ways that MSIX packaging can help you streamline and improve your app installation and deployment workflows.

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]