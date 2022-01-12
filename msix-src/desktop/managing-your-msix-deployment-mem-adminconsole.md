---
description: This guide provides instruction on how to create and deploy an MSIX app with Microsoft Endpoint Manager Admin Console
title: Microsoft Endpoint Manager Admin Console
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, mem admin console, app
ms.custom: RS5
---

# Microsoft Endpoint Manager Admin Console
The [Microsoft Endpoint Manager Admin Console](https://devicemanagement.microsoft.com) can be used to deploy an MSIX package to client devices. When deploying an MSIX app through the Microsoft Endpoint Configuration Manager admin console, you will require an MSIX app to deploy, as well as an Azure Active Directory Group to be the target of the install.

## Deploying MSIX Application
When deploying an App from within the Microsoft Endpoint Manager Admin Console to be delievered, you will first require a local or network path to your MSIX package. The details of the App being created from within the Microsoft Endpoint Manager Admin Console will retrieve the meta data contained within the MSIX app package and automatically load the retrieved information into the App properties.

| Task | Documentation reference |
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md)  |
| 2. Launch the Microsoft Endpoint Manager Admin Console | [Microsoft Endpoint Manager Admin Console](https://devicemanagement.microsoft.com) |
| 3. Create and deploy a Line-of Business App | [Create a Line-of Business App](/intune/apps/lob-apps-windows) |
