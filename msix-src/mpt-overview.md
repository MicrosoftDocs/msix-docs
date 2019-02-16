---
title: MSIX Packaging Tool Overview
description: Overview doc on getting started with Msix Packaging Tool
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX Packaging Tool 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool enables you to repackage your existing Win32 applications to the MSIX format. It offers both an interactive GUI and a command line for conversions, and gives you the ability to convert an application without having the source code. We want to enable IT Pros to convert their existing assets to MSIX, to give them a better way to do packaging and app management.

MSIX Packaging Tool is now available from the Microsoft Store. You can run your desktop installers through this tool and obtain an MSIX package that you can install on your machine.

## Prerequisites

- Windows 10, version 1809 (or later)
- Participation in the Windows Insider Program (if you're using an Insider build)
- A valid Microsoft account (MSA) alias to access the app from the Microsoft Store 
- Administrator privileges on your PC to run the tool
 
 ## Install
 
To install the MSIX Packaging Tool from the Microsoft Store, go [here](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), making sure you are logged in with the MSA that is used for your Windows Insider Program. Next, go to the product description page and click the Install icon to begin the installation.

MSIX Packaging tool can also be downloaded for offline use in the enterprise from Microsoft Store for Business [web portal](https://businessstore.microsoft.com/). You can learn more about offline distribution [here](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

 
 ## What's new:
 **v1.2019.110.0**
- Improved packaging times 
- Updated default file exclusion list
- Incorporated MSIExec error logs into tool reporting
- Updated logs to add more clarity and troubleshooting steps
- Added support for capturing installation from PowerShell ISE during manual packaging
- Added support for declaring PowerShell scripts as installer argument in the UI and the command line template file
- Added a verbose logging flag(--verbose | -v) for the Command line interface
- Fixed an issue where network paths on the VM were sometimes inaccessible
- Fixed an issue where Store versioning requirement validation was failing when using the command line interface
- Fixed an issue where file paths in quotations were not being accepted
- Fixed an issue where the VM was not being cleaned up correctly after conversion
- Fixed an issue where adding files to packages in package editor was not working properly
- UI cleanup 


 ## Tasks
 
Here is what you can expect to be able to do with this tool:
 
- Package your favorite application(msi, exe, App-V 5.x and to MSIX format by launching the tool and selecting the **Application package** icon.
- Create a modification package for a newly created Application MSIX Package by launching the tool and selecting the **Modification package** icon. 
- Open your MSIX package to view and edit its content/properties by navigating to the **Open package editor** tab, browsing to the MSIX package, and selecting **Open package**.

## MSIX Packaging Tool Insider Preview Program

Join the program to get all the latest MSIX Packaging tool fixes as soon as they are available and be one of the first to experience the new ideas and concepts we’re building. In return we want to know what you think.

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://aka.ms/MSIXPackagingPreviewProgram" data-linktype="external">Click here to join</a></p></div>
