---
description: This guide provides instruction on how to create and deploy an MSIX app with Microsoft Intune
title: Microsoft Intune
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp, mem, mem ac, mem admin console, app, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Microsoft Intune
The [Microsoft Intune Console](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) can be used to create a Line-of Business App which can be delivered to client devices. When deploying an MSIX app through the Microsoft Intune, you will require an MSIX app to deploy, as well as an Azure Active Directory Group to be the target of the install.

## Deploying an MSIX app
When deploying a client app from Microsoft Intune to be delivered to client devices you will first require a local or network path to your MSIX app. The details of the app being created from within the Microsoft Intune will retrieve the meta data contained within the MSIX app package and automatically load the retrieved information into the app properties.

|||
|-----|------|
| 1. Capture an MSIX Package from Installer, or retrieve an existing MSIX app | [Create an MSIX Package from desktop installer](../packaging-tool/create-app-package.md) |
| 2. Launch the Intune Console | [Microsoft Intune Console](https://portal.azure.com/#blade/Microsoft_Intune_DeviceSettings/ExtensionLandingBlade/overview) |
| 3. Create and deploy a Line-of Business App | [Create a Line-of Business App](/intune/apps/lob-apps-windows) |
