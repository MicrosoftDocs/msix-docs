---
title: MSIX Core
description: This article provides an overview of MSIX Core.
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---


# MSIX Core

MSIX Core brings MSIX support to Windows 7, Windows 8, and Windows 10 versions earlier than 1709. MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/preview/MSIXCore) on GitHub currently in preview, but it already enables these earlier Windows versions to install MSIX packages.

With MSIX Core, developers and IT pros who need to support users on these earlier Windows versions can now adopt and take advantage of the benefits of MSIX.

##  What is MSIX Core?

MSIX Core enables the installation of MSIX apps on previous versions of Windows, provided that the apps are already built to work on those versions of Windows. More specifically, MSIX Core is built for Windows versions that don't currently natively support MSIX: Windows 7 SP1, all versions of Windows 8, and Windows 10 versions prior to 1709 (Fall Anniversary Update).

MSIX Core is designed for both developers and IT pros. Developers can use the MSIX Core Library to enable their existing installers to install their MSIX packaged apps on previous Windows versions, so they can produce just one MSIX package to target all Windows users. IT pros can download the MSIX Core Installer to install on their users' machines and initiate silent installs of MSIX apps on those machines, which will enable users to install MSIX packages simply by double clicking them.

## Limitations of MSIX Core

The goal of MSIX Core is to enable the install, query, and removal of MSIX packaged apps (that already work on those Windows versions), and provide as clean of an uninstall as possible. The goal of MSIX Core is not to support all of the features of native MSIX. MSIX Core will not provide the container benefits of native MSIX, nor enable an app that uses Windows 10 specific features to work on previous Windows versions. 

MSIX Core also will not support Microsoft Store integration. Developers who want to publish their applications to the Microsoft Store can follow the documentation [here](https://docs.microsoft.com/windows/uwp/publish/).
