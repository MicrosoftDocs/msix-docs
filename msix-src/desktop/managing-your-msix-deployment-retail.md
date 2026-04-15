---
description: This article provides all the details you need to manage deploying you MSIX applications in an retail environment.  This article is targeted at developers.
title: Distribute your MSIX in a consumer environment
ms.date: 04/15/2026
ms.topic: article
keywords: windows 10, windows 11, deployment, msix
ms.assetid:  
---

# Distribute your MSIX in a consumer environment

If you are not an enterprise developer, retail distribution of MSIX can be done through normal retail channels.  This includes hosting the MSIX on a web page.  

## Microsoft Store

The [Microsoft Store](https://apps.microsoft.com/home) can be an excellent way to distribute your application.  It has been available to consumers since the launch of Windows 8 in late 2012. Users can browse, search and download apps or games for Windows.

For details on the Microsoft Store and its capabilities, see [Microsoft Store.](/windows/uwp/publish/) 

To get started on publishing an app to the Microsoft Store and its capabilities, see the [Partner Center](https://partner.microsoft.com/dashboard/home)

## CI/CD and beta distribution

To automate building, testing, and distributing your MSIX packages, use [GitHub Actions](/windows/msix/desktop/cicd-overview) or [Azure Pipelines](/azure/devops/pipelines/apps/windows/universal). Both support MSIX packaging and can distribute pre-release builds to testers via a shared network location, web install, or Microsoft Intune.

> [!NOTE]
> Visual Studio App Center was retired on March 31, 2025. See [MSIX and CI/CD Pipeline Overview](/windows/msix/desktop/cicd-overview) for the current guidance.

## Web Install

While a MSIX file can be hosted on any IIS server.  You can dramatically improve the experience by enabling direct install with App Installer.  Direct install will allow the AppInstaller to launch immediately and run the install from the web server rather than downloading it first.  For more details on Web install, see 
[Installing Windows 10 apps from a web page.](../app-installer/installing-windows10-apps-web.md)

