---
title: AppInstaller File Builder tool
description: This article provides details about the AppInstaller File Builder tool in the MSIX Toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msix toolkit
ms.custom: 19H1
---

# AppInstaller File Builder tool

The [AppInstaller File Builder tool](https://github.com/microsoft/MSIX-Toolkit/tree/master/AppInstallerFileBuilder) in the MSIX toolkit can be used to configure an MSIX-packaged app to check back to a central source for updates. This tool aids in the construction of an AppInstaller (.appinstaller) file. The AppInstaller file can be used with different [deployment options](../desktop/managing-your-msix-deployment-overview.md).

## Create an AppInstaller file

1. Download the [AppInstaller File Builder](https://github.com/microsoft/MSIX-Toolkit/releases/download/1.3.3/AppInstallerFileBuilder_1.2019.1001.0.msix)
2. Install and launch the **AppInstaller File Builder** app.
3. Select the **Get Package Info** button and browse to the MSIX app that you would like to install using the AppInstaller file.
4. Specify the update options that you would like the app package to have.
    > [!Note]
    > Options may vary based on selected minimum operating system version. **Check for updates on app launch** must be enabled within the tool to see or configure these values.
5. Continue through the wizard, adding [optional packages](../package/optional-packages.md), [modification packages](/windows/msix/modification-packages), [related packages](../package/optional-packages.md) and dependencies as required.
6. Select the **Generate** button after reviewing the Summary details to create the AppInstaller file.
