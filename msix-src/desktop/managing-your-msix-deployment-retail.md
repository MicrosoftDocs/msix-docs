---
description: This article provides all the details you need to manage deploying you MSIX applications in an retail environment.  This article is targeted at developers.
title: Distribute your MSIX in a consumer environment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Distribute your MSIX in a consumer environment

If you are not an enterprise developer, retail distribution of MSIX can be done through normal retail channels.  This includes hosting the MSIX on a web page.  

## Microsoft Store

The [Microsoft Store](https://www.microsoft.com/store/apps/windows) can be an excellent way to distribute your application.  It has been available to consumers since the launch of Windows 8 in late 2012. Users can browse, search and download apps or games for Windows.

For details on the Microsoft Store and its capabilities, see [Microsoft Store.](/windows/uwp/publish/) 

To get started on publishing an app to the Microsoft Store and its capabilities, see the [Partner Center](https://partner.microsoft.com/dashboard/home)

## App Center

[App Center](https://appcenter.ms/) enables you to automatically build your app, test it on real devices, and distribute it to beta testers.  App Center lets you ship apps more frequently, at higher-quality, and with greater confidence.  With App Center you can connect your repo and within minutes automate your builds, test on real devices in the cloud, distribute apps to beta testers, and monitor real-world usage with crash and analytics data. All in one place.
For more information on App Center see [App Center](/appcenter/).

## Web Install

While a MSIX file can be hosted on any IIS server.  You can dramatically improve the experience by enabling direct install with App Installer.  Direct install will allow the AppInstaller to launch immediately and run the install from the web server rather than downloading it first.  For more details on Web install, see 
[Installing Windows 10 apps from a web page.](../app-installer/installing-windows10-apps-web.md)

