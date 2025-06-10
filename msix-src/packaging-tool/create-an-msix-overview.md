---
title: Create an MSIX from an existing installer
description: Learn about the MSIX app packaging format and, how to prepare and migrate your applications to MSIX.
ms.date: 01/27/2020
ms.topic: concept-article
keywords: msix
---

# Overview

MSIX is the latest app packaging format from Microsoft that provides a cleaner and more reliable experience for app installation. To learn more about MSIX as a packaging format and all of its benefits, check out the MSIX Overview documentation. 

There are several paths when it comes to migrating your applications to MSIX, depending on what state they are in. This section of documentation will help you if you have existing installers that are either no longer in development, or you don't own the source code. If you do own the source code, check out these docs on [how to generate an MSIX from source code](../desktop/source-code-overview.md).

This section of documentation will show you how to set up your conversion environment, how to acquire and set up the MSIX Packaging Tool to set yourself up for success, and how to use the MSIX Packaging Tool to take any installer you have and convert it to MSIX. Once you've got an MSIX, we've also got information on how to fix runtime issues using the Package Support Framework.

 ## How to move your existing installers to MSIX

 When preparing to convert you application to MSIX, we recommend going through the following process to ensure that you get a clean MSIX package that is ready to be deployed. Before starting any of these steps, you may want to first [evaluate your installer](know-your-installer.md) to ensure it will be supported in MSIX format.

 ### Prepare your environment
 There are several ways to set up your environment for conversion. Start with the best practices for our recommendations.

 - [Best practices for setting up a conversion environment](prepare-your-environment.md)
 - [How to get a Quick Create VM](Quick-Create-VM.md)
 - [Setting up your machine for remote conversions](remote-conversion-setup.md)
 - [Using the MSIX Packaging Tool in a disconnected environment](disconnected-environment.md)

 ### Set up your conversion tool and settings

 - [MSIX Packaging Tool Overview](tool-overview.md)
 - [Best Practices for the MSIX Packaging Tool](tool-best-practices.md)
 - [Known issues and troubleshooting tips for the MSIX Packaging Tool](tool-known-issues.md)
 - [Duplicate MSIX Packaging Tool settings across devices](duplicate-tool-settings-across-devices.md)
 - [MSIX Packaging Tool Insider Program](insider-program.md)
 - [Release notes for the MSIX Packaging Tool](release-notes/history.md)

 ### Using the MSIX Packaging tool

 - [Create an MSIX package from a desktop installer (msi, exe, ClickOnce, or App-V)](create-app-package.md)
 - [Conversion with the command line](package-conversion-command-line.md)
 - [Support for desktop installers that require device restarts](support-restart.md)
 - [Convert an installer that includes services](convert-an-installer-with-services.md)
 - [Modify a package using Package Editor](package-editor.md)
 - [Edit icons and assets using the MSIX Packaging Tool](edit-icons-and-assets.md)

 ### Applying runtime fixes with the Packaging Support Framework

 - [Package Support Framework Overview](../psf/package-support-framework-overview.md)
 - [Fix issues that prevent your desktop application from running in an MSIX container](../psf/package-support-framework.md)
 - [Run scripts with the Package Support Framework](../psf/run-scripts-with-package-support-framework.md)

 Once you've got your MSIX package, you will be ready to move on to testing and validation of your application. 
