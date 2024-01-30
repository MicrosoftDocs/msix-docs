---
title: MSIX Packaging Tool Overview
description: This article introduces the MSIX Packaging Tool, which enables you to repackage your existing Windows desktop applications to the MSIX format.
ms.date: 04/28/2021
ms.topic: article
keywords: windows 10, uwp, msix
ms.custom: "RS5, seodec18"
---

# MSIX Packaging Tool 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool enables you to repackage your existing desktop applications to the MSIX format. It offers both an interactive UI and a command line for conversions, and gives you the ability to convert an application without having the source code. We want to enable IT Pros to convert their existing assets to MSIX, to give them a better way to do packaging and app management.

MSIX Packaging Tool is now available from the Microsoft Store. You can run your desktop installers through this tool and obtain an MSIX package that you can install on your machine.

If you are interested in being an MSIX Packaging Tool insider, click [here](insider-program.md) for more details.

## Prerequisites

- Windows 10, version 1809 (or later)
- Participation in the Windows Insider Program (if you're using an Insider build)
- A valid Microsoft account (MSA) alias to access the app from the Microsoft Store 
- Administrator privileges on your PC to run the tool
 
 ## Install
 
To install the MSIX Packaging Tool from the Microsoft Store, go [here](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf), making sure you are logged in with the MSA that is used for your Windows Insider Program. Next, go to the product description page and click the Install icon to begin the installation.

MSIX Packaging Tool can be installed from WinGet command-line tool using the command -
```
PS C:\> winget install "MSIX Packaging Tool"
```

MSIX Packaging tool can also be directly downloaded for offline use: 

- [Download 1.2023.1212.0 MSIX Packaging Tool](https://download.microsoft.com/download/6/c/7/6c7d654b-580b-40d4-8502-f8d435ca125a/da97fb568eee4e6baa07bc3b234048b3.msixbundle)  
You can learn more about using the MSIX Packaging Tool in a [disconnected environment here](/windows/msix/packaging-tool/disconnected-environment).

After you have the offline version of the application, you can use [PowerShell](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps&preserve-view=true) to add the app package and license to your machine. 

### Example of offline installation
```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

## Latest public release - Version 1.2023.1212.0

- Enhanced support to generate accelerator template with pre-filled fixes in Package Analyzer
- Added provision for Deletion markers via Registry Legacy in PSF Fixups
- Added support for Package Analyzer Working directory FixUp
- Support for Desktop Shortcuts with WinAppSDK 1.4.2

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
| [Create MSIX package from a MSI/App-V file](./create-app-package.md) | This tutorial will go through how to use MSIX Packaging Tool's UI to convert your desktop applications(particularly installers like MSI, EXE or App-V) to a MSIX Package. |
| [Create MSIX package using the command line](package-conversion-command-line.md) | This tutorial will go through how to use MSIX Packaging Tool and the command line to convert your desktop application to a MSIX Package. |
| [Create MSIX package on a remote device](remote-conversion-setup.md) | This article will provide the instructions required to perform the conversion of desktop applications to MSIX packages on a remote device. |

