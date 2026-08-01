---
title: Install Windows apps with App Installer
description: This section contains or links to articles about App Installer and how to use the features of App Installer.
ms.date: 07/04/2026
ms.topic: install-set-up-deploy
keywords: windows 10, windows 11, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.custom: "RS5, seodec18"
---

# Install Windows apps with App Installer

## Purpose
This section contains or links to articles about App Installer and how to use the features of App Installer.

App Installer allows for Windows apps to be installed by double clicking the package. This means that users don't need to use PowerShell or other developer tools to deploy Windows apps. The App Installer can also install an app from the web, optional packages, and related sets.

App Installer is built into Windows 10 (version 1803 and later) and Windows 11, and is available from the [Microsoft Store](https://apps.microsoft.com/detail/9nblggh4nns1).

> [!NOTE]
> Two related but distinct technologies share the "App Installer" name:
> - [App Installer](install-update-app-installer.md) (the app) is the built-in Windows application, available from the Microsoft Store, that provides the installation UI when a user double-clicks a package or an `.appinstaller` file.
> - The [App Installer file](app-installer-file-overview.md) (`.appinstaller`) is an XML file that specifies your package location and defines how the package is installed, updated, and repaired.

To learn how to use the App Installer to install your app, see the topics in the table.

| Topic | Description |
|-------|-------------|
| [App Installer user interface](app-installer-ui-dialog.md) | Understand the various components of the default App Installer interface. |
| [App Installer file overview](app-installer-file-overview.md) | Learn about the contents of App Installer files and how they work. |
| [Create an App Installer file in Visual Studio](create-appinstallerfile-vs.md)| Learn how to use Visual Studio to enable automatic updates using the .appinstaller file. |
| [Create an App Installer file manually](how-to-create-appinstaller-file.md)| Learn how to create an .appinstaller file manually. This is particularly useful for installing a related set that contains a main package and optional packages. |
| [Configure update settings in the App Installer file](update-settings.md)  |  Learn how to configure app updates by using the App Installer file. |
| [Install a Windows 10 app from the web](installing-windows10-apps-web.md) | In this section, we will review the steps you need to take to allow users to install your apps directly from the web page. |
| [Embeded App Installer File](how-to-embed-an-appinstaller-file.md) | Learn how to embed your App Installer file into your Windows apps |
| [Optional packages and related sets](../package/optional-packages.md) | Learn about related sets that contain a main package and related optional packages.  |
| [Troubleshoot installation issues with the App Installer file](troubleshoot-appinstaller-issues.md) | Common issues and solutions when sideloading applications with the App Installer file. |
| [Related documentation](app-installer-documentation.md) | Provides links to related documentation, including APIs that you can use to modify packages via App Installer files or to retrieve information about apps with an App Installer association.  |
| [App Installer file (.appinstaller) reference](/uwp/schemas/appinstallerschema/app-installer-file?context=%252fwindows%252fmsix%252frender) | View the full XML schema for the App Installer file. |
| [App Installer security features](app-installer-security-features.md) | Describes security features associated with the App Installer.|

## Tutorials

Follow these tutorials and learn how to host and install a Windows app from various distribution platforms. These tutorials are useful for enterprises and developers that don't want or need to publish their apps to the Store, but still want to take advantage of the Windows packaging and deployment platform.

| Tutorial | Description |
|----------|-------------|
| [Install a Windows app from an Azure Web App](web-install-azure.md) | Create an Azure Web App and use it to host and distribute your Windows app package. |
| [Install a Windows app from an IIS server](web-install-IIS.md) | Set up an IIS server, verify that your web app can host app packages, and use App Installer effectively. |
| [Hosting Windows app packages on AWS for web install](web-install-aws.md) | Learn how to set up Amazon Simple Storage Service to host your Windows app package from a web site. |
