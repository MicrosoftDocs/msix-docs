---
title: MSIX Container 
description: This article describes the MSIX Container
ms.date: 02/04/2020
ms.topic: article
author: dianmsft
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
--- 

# MSIX Container

Apps that are packaged using MSIX run in a lightweight app container. The MSIX app process and its child processes run inside the container and are isolated using file system and registry virtualization. All MSIX apps can read the global registry. An MSIX app writes to its own virtual registry and application data folder, and this data will be deleted when the app is uninstalled or reset. Other apps do not have access to the virtual registry or virtual file system of an MSIX app.

For more information, visit [Understanding how MSIX packages run on Windows](desktop/desktop-to-uwp-behind-the-scenes.md). 
