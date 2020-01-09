---
title: MSIX Core
description: This article provides an overview of MSIX Core, which brings MSIX support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update).
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Core

MSIX Core brings MSIX support to versions of Windows earlier than Windows 10, version 1709. MSIX Core is an [open source project](https://github.com/Microsoft/msix-packaging/tree/master/MsixCore) on GitHub that enables these earlier Windows versions to install MSIX packages. You can start by downloading the [latest release](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-1.1-release) or the [preview builds](https://github.com/microsoft/msix-packaging/releases/tag/MSIX-Core-preview).

With MSIX Core, developers and IT pros who need to support users on these earlier Windows versions can now adopt and take advantage of the benefits of MSIX.

## What is MSIX Core?

MSIX Core enables the installation of MSIX apps on previous versions of Windows, provided that the apps are already built to work on those versions of Windows. MSIX Core is built for the following Windows versions that don't currently natively support MSIX:

* Windows 7 SP1
* Windows 8.1
* Currently supported Windows Server (with Desktop Experience)
* Windows 10 versions prior to 1709

MSIX Core is designed for both developers and IT pros. Developers can use the MSIX Core Library to enable their existing installers to install their MSIX packaged apps on previous Windows versions, so they can produce just one MSIX package to target all Windows users. IT pros can download the MSIX Core Installer.  MSIX Core installer enables command line installation of MSIX along with the ability for users to install MSIX packages simply by double clicking them.

## Considerations of MSIX Core

The goal of MSIX Core is to enable the install, query, and removal of MSIX packaged apps (that already work on those Windows versions), and provide as clean of an uninstall as possible. MSIX Core provides a subset of features of native MSIX, functioning similar to existing Win32 installer types.

* MSIX Core does not provide the container benefits of native MSIX, nor enable an app that uses Windows 10 specific features to work on previous Windows versions.
* When using MSIX Core on a down-level OS, [app execution aliases](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias) will only work from **Win+R** and not from Command Prompt or PowerShell.
* MSIX Core does not support Microsoft Store integration. Developers who want to publish their applications to the Store can follow the documentation [here](https://docs.microsoft.com/windows/uwp/publish/).

## Get started

To deploy your MSIX package with MSIX Core, you must first [update your existing MSIX manifest](support-msix-core.md). Then, you can [deploy your MSIX package with MSIX Core](deploy-with-msix-core.md) (if you only have the package) or you can [create an MSIX package with MSIX Core from source code](msixcore-clickonce-solution.md) (if you have the source code).
