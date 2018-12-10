---
author: mcleanbyron
title: Install Windows 10 apps with App Installer | Microsoft Docs
description: This section contains or links to articles about App Installer and how to use the features of App Installer.
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.localizationpriority: medium
ms.custom: RS5
---

# Install Windows 10 apps with App Installer

## Purpose
This section contains or links to articles about App Installer and how to use the features of App Installer. 

App Installer allows for Windows 10 apps to be installed by double clicking the app package. This means that users don't need to use PowerShell or other developer tools to deploy Windows 10 apps. The App Installer can also install an app from the web, optional packages, and related sets. To learn how to use the App Installer to install your app, see the topics in the table.

| Topic | Description |
|-------|-------------|
| [Create App Installer file with Visual Studio](create-appinstallerfile-vs.md)| Learn how to use Visual Studio to enable automatic updates using the .appinstaller file. |
| [Install Windows 10 apps from a web page](installing-windows10-apps-web.md) | In this section, we will review the steps you need to take to allow users to install your apps directly from the web page. |
| [Install a related set using an App Installer file](install-related-set.md) | In this section, learn how to allow the installation of a related set via App Installer. We will also go through the steps to construct an App Installer file that will define your related set. |
| [How to manually create an AppInstaller file](how-to-create-appinstaller-file.md) | In this section, learn how to construct an App Installer file. |
| [Troubleshoot installation issues with the App Installer file](troubleshoot-appinstaller-issues.md) | Common issues and solutions when sideloading applications with the App Installer file. |
| [App Installer file (.appinstaller) reference](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file?context=/windows/msix/render) | View the full XML schema for the App Installer file. |

## Tutorials 

Follow these tutorials and learn how to host and install a Windows 10 app from various distribution platforms. These tutorials are useful for enterprises and developers that don't want or need to publish their apps to the Store, but still want to take advantage of the Windows 10 packaging and deployment platform.

| Tutorial | Description |
|----------|-------------|
| [Install a Windows 10 app from an Azure Web App](web-install-azure.md) | Create an Azure Web App and use it to host and distribute your Windows 10 app package. |
| [Install a Windows 10 app from an IIS server](web-install-IIS.md) | Set up an IIS server, verify that your web app can host app packages, and use App Installer effectively. |
| [Hosting Windows 10 app packages on AWS for web install](web-install-aws.md) | Learn how to set up Amazon Simple Storage Service to host your Windows 10 app package from a web site. |
