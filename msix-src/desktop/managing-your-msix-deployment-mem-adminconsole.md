---
description: This guide provides instruction on how to create and deploy an MSIX app with the Microsoft Intune admin center
title: Microsoft Intune admin center
ms.date: 04/15/2026
ms.topic: article
keywords: windows 10, intune, admin center, app, msix
---

# Microsoft Intune admin center
The [Microsoft Intune admin center](https://intune.microsoft.com/) can be used to deploy an MSIX package to client devices. When deploying an MSIX app through the Microsoft Intune admin center, you will require an MSIX app to deploy, as well as a Microsoft Entra ID group to be the target of the install.

## Deploying an MSIX application
When deploying an app from within the Microsoft Intune admin center, you will first require a local or network path to your MSIX package. The details of the app being created from within the Microsoft Intune admin center will retrieve the metadata contained within the MSIX app package and automatically load the retrieved information into the app properties.

| Task | Documentation reference |
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md)  |
| 2. Launch the Microsoft Intune admin center | [Microsoft Intune admin center](https://intune.microsoft.com/) |
| 3. Create and deploy a Line-of-Business App | [Create a Line-of-Business App](/mem/intune/apps/lob-apps-windows) |
