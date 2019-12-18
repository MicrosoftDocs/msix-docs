---
title: MSIX Core
description: This article provides an overview of MSIX Core, which brings MSIX support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update).
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Core

MSIX Core brings MSIX support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update). MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) on GitHub that enables these earlier Windows versions to install MSIX packages. You can start by downloading the [latest release](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) or the [preview builds](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview).

With MSIX Core, developers and IT pros who need to support users on these earlier Windows versions can now adopt and take advantage of the benefits of MSIX.

##  What is MSIX Core?

MSIX Core enables the installation of MSIX apps on previous versions of Windows, provided that the apps are already built to work on those versions of Windows. More specifically, MSIX Core is built for Windows versions that don't currently natively support MSIX: Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update).

MSIX Core is designed for both developers and IT pros. Developers can use the MSIX Core Library to enable their existing installers to install their MSIX packaged apps on previous Windows versions, so they can produce just one MSIX package to target all Windows users. IT pros can download the MSIX Core Installer.  MSIX Core installer enables command line installation of MSIX along with the ability for users to install MSIX packages simply by double clicking them.

## Considerations of MSIX Core

The goal of MSIX Core is to enable the install, query, and removal of MSIX packaged apps (that already work on those Windows versions), and provide as clean of an uninstall as possible. MSIX Core provides a subset of features of native MSIX, functioning similar to existing Win32 installer types. MSIX Core does not provide the container benefits of native MSIX, nor enable an app that uses Windows 10 specific features to work on previous Windows versions. 

MSIX Core also will not support Microsoft Store integration. Developers who want to publish their applications to the Microsoft Store can follow the documentation [here](https://docs.microsoft.com/windows/uwp/publish/).

### Get Started 
In order to deploy your MSIX packages to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update) with MSIX Core, you will need to [update your existing MSIX manifest](https://docs.microsoft.com/windows/msix/msix-core/support-msix-core). 

To leverage the benefits of MSIX on Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update), [deploy MSIX packages with MSIX Core](https://docs.microsoft.com/windows/msix/msix-core/deploy-with-msix-core) or if you have the source code, [creating MSIX package with MSIX Core from your source code](https://docs.microsoft.com/windows/msix/msix-core/msixcore-clickonce-solution).