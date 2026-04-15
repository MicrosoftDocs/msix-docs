---
description: This guide provides instruction on how to create and deploy an MSIX app with Microsoft Configuration Manager
title: Microsoft Configuration Manager
ms.date: 04/15/2026
ms.topic: article
keywords: windows 10, configmgr, app, msix
---

# Microsoft Configuration Manager
The [Microsoft Configuration Manager](/configmgr/) can be used to create a `Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)` which can be delivered to client devices. Deploying an MSIX app has the following prerequisites:
1) Access to the MSIX package to deploy.
2) A device or user collection to be the target of the install.

## Deploying an MSIX application
When deploying a Windows app package from within the Microsoft Configuration Manager console, you will first require a UNC path to your MSIX app package. The details of the app being created will be retrieved from the metadata contained within the MSIX app package and automatically added into the Windows app package properties.

| Task | Documentation reference |
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md)  |
| 2. Launch the Microsoft Configuration Manager console | [Microsoft Configuration Manager](https://www.microsoft.com/microsoft-365/microsoft-endpoint-manager) |
| 3. Create a Line-of-Business App | [Create a Windows app package](/configmgr/apps/get-started/creating-windows-applications) |
| 4. Create a user or device collection | [Creating collections](/configmgr/core/clients/manage/collections/create-collections) |
| 5. Deploy the Line-of-Business App | [Deploying applications with Configuration Manager](/configmgr/apps/deploy-use/deploy-applications) |
