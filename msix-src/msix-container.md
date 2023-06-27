---
title: MSIX appContainer apps
description: This topic describes MSIX appContainer apps.
ms.date: 06/27/2023
ms.topic: article
author: dianmsft
keywords: windows 11, windows 10, uwp, msix
ms.custom: RS5
--- 

# MSIX appContainer apps

If you have an app that's packaged using MSIX, then you can configure it to run in a lightweight app container. Universal Windows Platform (UWP) apps are *appContainer* apps. And you can also configure a desktop app (if it's packaged with MSIX) to be an *appContainer* app. An *appContainer* app's process and its child processes run inside the container; and they're isolated using file system and registry virtualization (all MSIX apps can *read* the global registry).

## Configure as appContainer

To configure your packaged app as an *appContainer* app, set the **uap10:TrustLevel** attribute (on the **Application** element in your app package manifest):

* To declare an *appContainer* app, set `uap10:TrustLevel="appContainer"`.
* Conversely, a *full trust* app is declared with `uap10:TrustLevel="mediumIL"`.

For more info, see these topics:

* [Application element](/uwp/schemas/appxpackage/uapmanifestschema/element-application)
* [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041)
* [Types of desktop app](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#types-of-desktop-app)

## The purpose of an app container

A key goal of an *appContainer* app is to separate app state from system state as much as possible, while maintaining compatibility with other apps. Windows accomplishes that by detecting and redirecting certain changes that it makes to the file system and registry at runtime (known as *virtualizing*).

An *appContainer* app writes to its own virtual registry and application data folder, and that data is deleted when the app is uninstalled or reset. Other apps don't have access to the virtual registry or virtual file system of an *appContainer* app.
