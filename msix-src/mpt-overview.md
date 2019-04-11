---
title: MSIX Packaging Tool Overview
description: Overview doc on getting started with Msix Packaging Tool
author: mcleanbyron
ms.author: mcleans
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Packaging Tool 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool enables you to repackage your existing Win32 applications to the MSIX format. It offers both an interactive GUI and a command line for conversions, and gives you the ability to convert an application without having the source code. We want to enable IT Pros to convert their existing assets to MSIX, to give them a better way to do packaging and app management.

MSIX Packaging Tool is now available from the Microsoft Store. You can run your desktop installers through this tool and obtain an MSIX package that you can install on your machine.

Interested to be a MSIX Packaging Tool insider, click [here](packaging-tool/insider-program.md) for more details.

## Prerequisites

- Windows 10, version 1809 (or later)
- Participation in the Windows Insider Program (if you're using an Insider build)
- A valid Microsoft account (MSA) alias to access the app from the Microsoft Store 
- Administrator privileges on your PC to run the tool
 
 ## Install
 
To install the MSIX Packaging Tool from the Microsoft Store, go [here](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), making sure you are logged in with the MSA that is used for your Windows Insider Program. Next, go to the product description page and click the Install icon to begin the installation.

MSIX Packaging tool can also be downloaded for offline use in the enterprise from Microsoft Store for Business [web portal](https://businessstore.microsoft.com/). You can learn more about offline distribution [here](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

 
## Latest public version - 1.2019.402.0

### New features:

- Ability to convert on a remote machine - [more info](remote-conversion-setup.md)
- Improved management experience in package editor
    - Auto versioning recommendations when saving in package editor
    - Now supports existing folder addition to package in VFS
- User can specify known valid exit codes for CLI conversions
- Added the ability to time stamp your signed package in all of the workflows where signing is currently available 
    - You can specify your default time stamp URL and type of time stamp server in the tool Settings page
- Updated [AppID generation logic](release-notes/history.md#appid-generation-logic), and added additional validation for package name and app 

You can find the full history of MSIX Packaging Tool release notes [here](release-notes/history.md).

 ## Tasks
 
Here is what you can expect to be able to do with this tool:
 
- Package your favorite application(msi, exe, App-V 5.x and to MSIX format by launching the tool and selecting the **Application package** icon.
- Create a modification package for a newly created Application MSIX Package by launching the tool and selecting the **Modification package** icon. 
- Open your MSIX package to view and edit its content/properties by navigating to the **Open package editor** tab, browsing to the MSIX package, and selecting **Open package**.

