---
Description: This guide provides instruction on how to create and deploy an MSIX app with Microsoft Endpoint Configuration Manager
title: Microsoft Endpoint Configuration Manager
ms.date: 1/31/2020
ms.topic: article
keywords: windows 10, uwp, mecm, configmgr, app, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Microsoft Endpoint Configuration Manager
The [Microsoft Endpoint Configuration Manager](/configmgr/) can be used to create a `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)` which can be delivered to client devices. Deploying a MSIX app has the following prerequisites:
1) Access to the MSIX package to deploy.
2) a device or user collection to be the target of the install.

## Deploying MSIX Application
When deploying an Windows app package from within the Microsoft Endpoint Configuration Manager Console to be delievered, you will first require a UNC path to your MSIX app package. The details of the App being created will be retrieved from the meta data contained within the MSIX app package and automatically added into the Windows app package properties.

|||
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md)  |
| 2. Launch the Microsoft Endpoint Configuration Manager Console | [Microsoft Endpoint Configuration Manager Console](https://devicemanagement.microsoft.com) |
| 3. Create a Line-of Business App | [Create a Windows app package](/configmgr/apps/get-started/creating-windows-applications) |
| 4. Create a user or device collection | [Creating collections](/configmgr/core/clients/manage/collections/create-collections) |
| 5. Deploy the Line-of Business App | [Deploying applications with ConfigMgr](/configmgr/apps/deploy-use/deploy-applications) |