---
title: Install and update the App Installer
description: How to install and update the App Installer
ms.date: 12/20/2023
ms.topic: article
keywords: windows 11, windows 10, uwp, app installer, AppInstaller, install, update
---

# Installing the App Installer

The App Installer ships as part of Windows. It allows for easy installation of MSIX files and MSIXBundles.

If updates are enabled on a PC, then the App Installer will be updated automatically from the Microsoft Store. If the App Installer doesn't update, then you can use the troubleshooting tips at [Fix problems with apps from the Microsoft Store](https://support.microsoft.com/account-billing/fix-problems-with-apps-from-microsoft-store-93ed0bcf-9c12-3df6-6dda-92ec5d0415ac).

If your PC doesn't have access to the Microsoft Store, then use the download button below

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Download the installer for the latest version of App Installer](https://aka.ms/getwinget)

## Use the winget tool to update

Winget can update App Installer for you. At the command prompt, just type `winget update winget`.

## Which version do I have?

You can use this PowerShell command to determine the version of App Installer that you have installed:

```powershell
(Get-AppxPackage Microsoft.DesktopAppInstaller).Version
```
