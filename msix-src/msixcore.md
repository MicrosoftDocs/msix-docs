---
author: andyliumsa
title: MSIX Core
description: This article summarizes the use .
ms.author: liandy
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---


# MSIX Core

MSIX Core enlightens previous Windows OS's with MSIX support. MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/preview/MSIXCore) on Github currently in preview, but can already enable earlier Windows OS's to install MSIX packages. 

With MSIX Core, developers and IT pros who need to support users on Windows 7, Windows 8, and Windows 10 versions earlier than 1709 can now also adopt and take advantage of the benefits of MSIX.

##  What is MSIX Core?

MSIX Core enables the installation of MSIX apps on previous versions of Windows OS's, provided that those apps are already built to work on those versions of Windows. More specifically, MSIX Core is built for Windows 7 SP1, all versions of Windows 8, and Windows 10 versions prior to 1709 (Fall Anniversary Update).

MSIX Core is designed for both developers and IT pros. Developers can use the MSIX Core Library to enable their existing installers to install their MSIX packaged apps on previous Windows OS's, so a developer can produce just one MSIX package to target all Windows users. IT pros can download the MSIX Core Installer MSI to install on their users' machines, which will enable users to double click on MSIX packages to install and IT pros to initiate silent installs of MSIX apps on those machines.

## Limitations of MSIX Core

The goal of MSIX Core is to enable the install, query, and removal of MSIX packaged apps (that already work on those Windows versions), and provide as clean of an uninstall as possible. The goal of MSIX Core is not to support all of the features of native MSIX. MSIX Core will not provide the container benefits of native MSIX, nor enable an app that uses Windows 10 specific features to work on previous Windows OS's. 

