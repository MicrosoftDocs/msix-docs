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

MSIX enables enterprises to stay current and ensure their applications are always up to date. It allows IT Pros and developers to deliver a user centric solution while stile reducing the cost of ownership of application by reducing the need to repackage. 

## Reliability with MSIX 
MSIX provides a reliable install. MSIX has a 99.96% success rate for installs over millions of installs. There is also a 100% guaranteed uninstalled. MSIX improves network bandwidth by only downloading what is needed. MSIX is designed for modern systems and the cloud. With MSIX there is no duplication of files across apps and Windows manages the shared files across apps.

## What makes up an MSIX package 
Here are a list of required components needed to create and install an MSIX Package. 

| Name | Description |
|------|---------|
| AppxBlockMap.xml | The package block map file is an XML document that contains a list of the app’s files along with indexes and cryptographic hashes for each block of data that is stored in the package. The block map file itself is verified and secured with a digital signature when the package is signed. The block map file allows MSIX packages to be downloaded and validated incrementally, and also works to support diff updates to the app files after they’re installed.| 
| AppxManifest.xml | The package manifest is an XML document that contains the info the system needs to deploy, display, or update a Windows app. This info includes package identity, package dependencies, required capabilities, visual elements, and extensibility points. | 
| AppxSignature.p7x | Generated when the package is signed. All MSIX Package are required to be signed before install. |

## MSIX Container 
All MSIX apps share the global registry for reading. An MSIX package will write to its own virtual registry and App Data folder. This will be deleted when the app is uninstall or a reset. Other apps will not have access to it's virtual registry or virtual file system. 

## Highlights of MSIX

* **Package existing Windows apps.** Use the [MSIX Packaging Tool](packaging-tool/mpt-overview.md) to create an MSIX package for any Windows app, old or new. The MSIX packaging tool streamlines the packaging experience, offering an interactive user interface or command line to convert and package Windows apps.
* **Install MSIX app packages.** Use [App Installer](app-installer/app-installer-root.md) to install or update any MSIX app package that is locally available or on any content distribution network.
* **Apply run time fixes to packaged apps.** The [Package Support Framework](psf/package-support-framework-overview.md) is an open source kit that helps you apply fixes to your existing desktop app when you don't have access to the source code, so that it can run in an MSIX container.
* **Use MSIX anywhere.** With the open source [MSIX SDK](msix-sdk/sdk-overview.md), MSIX packages are more versatile, and platform independent. The SDK provides all of the APIs needed to verify, validate, and unpack an app package on any platform, including Windows 10 and non-Windows 10 platforms.

> [!TIP]
> Visit the [MSIX Tech Community](https://aka.ms/msixcommunity) page for discussions and latest information.

This video introduces the key ways that MSIX packaging can help you streamline and improve your app installation and deployment workflows.

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]
