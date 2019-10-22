---
title: MSIX Packaging Tool Overview
description: Overview doc on getting started with Msix Packaging Tool
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Packaging Tool 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool enables you to repackage your existing Win32 applications to the MSIX format. It offers both an interactive UI and a command line for conversions, and gives you the ability to convert an application without having the source code. We want to enable IT Pros to convert their existing assets to MSIX, to give them a better way to do packaging and app management.

MSIX Packaging Tool is now available from the Microsoft Store. You can run your desktop installers through this tool and obtain an MSIX package that you can install on your machine.

Interested to be a MSIX Packaging Tool insider, click [here](insider-program.md) for more details.

## Prerequisites

- Windows 10, version 1809 (or later)
- Participation in the Windows Insider Program (if you're using an Insider build)
- A valid Microsoft account (MSA) alias to access the app from the Microsoft Store 
- Administrator privileges on your PC to run the tool
 
 ## Install
 
To install the MSIX Packaging Tool from the Microsoft Store, go [here](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), making sure you are logged in with the MSA that is used for your Windows Insider Program. Next, go to the product description page and click the Install icon to begin the installation.

MSIX Packaging tool can also be downloaded for offline use in the enterprise from Microsoft Store for Business [web portal](https://businessstore.microsoft.com/). You can learn more about offline distribution [here](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

 
## Latest public version - 1.2019.1018.0

### New features:
- Device Guard signing is now available. This signing option requires an active Microsoft Azure Active Directory account configured for the Microsoft Store for Business. For more information, see [this article](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing).
- The **Package editor** now supports the ability to select multiple items on which to perform an action.
- Right-click to edit an MSIX package is now supported.
- Ux improvements for the packaging workflow.

You can find the full history of MSIX Packaging Tool release notes [here](release-notes/history.md).

 ## Tasks
 
Here is what you can expect to be able to do with this tool:
 
- Package your favorite application(msi, exe, App-V 5.x and custom scripts) in the MSIX format by launching the tool and selecting the **Application package** icon.
- Create a modification package for a MSIX Package by launching the tool and selecting the **Modification package** icon. 
- Open your MSIX package to view and edit its content/properties by selecting the **Package editor** icon, browsing to the MSIX package, and selecting **Open package**.

## Try it out 

The following articles are tutorials on how to use MSIX Packaging Tool to convert your desktop applications: 

| Article | Description |
|-------|-------------|
| [Create MSIX package from a MSI/App-V file](create-app-package-MSI-VM.md) | This tutorial will go through how to use MSIX Packaging Tool's UI to convert your desktop applications(particularly installers like MSI, EXE or App-V) to a MSIX Package. |
| [Create MSIX package from other installer type](create-other-installer.md) | This tutorial will go through how to use MSIX Packaging Tool's UI to convert your desktop application(installers like batch scripts, PowerShell etc) to a MSIX Package. |
| [Create MSIX package using MSIX Packaging Tool's command line interface](package-conversion-cli.md) | This tutorial will go through how to use MSIX Packaging Tool's command line interface to convert your desktop application to a MSIX Package. |
| [Automate MSIX package conversion](automate-conversion.md) | This tutorial will discuss how you can use the command line interface to automate the conversion of desktop applications to MSIX Packages. |
| [Create MSIX package on a remote device](remote-conversion-setup.md) | This article will provide the instructions required to perform the conversion of desktop applications to MSIX packages on a remote device. |
