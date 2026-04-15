---
description: This guide provides instruction on how to create and deploy an MSIX app with Microsoft Intune
title: Microsoft Intune
ms.date: 04/15/2026
ms.topic: article
keywords: windows 10, uwp, intune, app, msix
---

# Microsoft Intune
The [Microsoft Intune admin center](https://intune.microsoft.com/) can be used to create a Line-of-Business App which can be delivered to client devices. When deploying an MSIX app through Microsoft Intune, you will require an MSIX app to deploy, as well as a Microsoft Entra ID group to be the target of the install.

## Deploying an MSIX app
When deploying a client app from Microsoft Intune to be delivered to client devices you will first require a local or network path to your MSIX app. The details of the app being created from within the Microsoft Intune admin center will retrieve the metadata contained within the MSIX app package and automatically load the retrieved information into the app properties.

| Task | Documentation reference |
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md) |
| 2. Launch the Intune admin center | [Microsoft Intune admin center](https://intune.microsoft.com/) |
| 3. Create and deploy a Line-of-Business App | [Create a Line-of-Business App](/mem/intune/apps/lob-apps-windows) |
