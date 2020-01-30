---
title: MSIX Modify Package Publisher
description: This article provides details about the available MSIX package publisher Script.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msix toolkit
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX App Installer
## SYNOPSIS
The MSIX AppInstaller file can be used to configure an MSIX app to check back to a central source for updates. This tool aids in the construction of an *.AppInstaller file. The AppInstaller file can be used with different [deployment options](../desktop/managing-your-msix-deployment.md).

## Create an AppInstaller file:

1.  Download the [AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/releases/download/1.3.3/AppInstallerFileBuilder_1.2019.1001.0.msix)

1.  Install and launch the **AppInstaller File Builder** app.

1.  Select the **Get Package Info** button and browse to the MSIX app that you would like to install using the AppInstaller file.

1.  Specify the update options that you would like the app package to have.

    > [!Note]    
    > - Options may vary based on selected minimum operating system version. 
    > - **Check for updates on app launch** must be enabled within the tool to see or configure these values.

1.  Continue through the wizard, adding [Optional Packages](../package/optional-packages.md), [Modification Packages](//modification-packages.md), [Related Packages](../package/optional-packages.md) and [Dependencies]() are required.

1.  Select the **Generate** button after reviewing the Summary details to create the AppInstaller file.