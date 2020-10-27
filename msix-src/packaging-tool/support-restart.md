---
title: Support for desktop installers that require device restart
description: Describes how to support desktop installers that require device restart.
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
---

# Support for desktop installers that require device restart

Beginning in the 1.2019.701.0 version of the MSIX Packaging Tool, you can trigger a restart during conversion for your installer.

Restarts are supported both through our interactive UI and through the command line. In addition, through the command line, you can also specify you want to auto-login after the restart to have a seamless hands-free conversion. 

You can specify which exit codes indicate a restart in the Settings > [Installer exit codes](tool-best-practices.md#other-settings) page. 

You can trigger a restart through the UI during your conversion workflow or through the start menu of your conversion machine. After the restart has happened, you will return to the same place you were in the tool to continue with your conversion.

We always want to hear from you, so if you have any feedback, please be sure to [file feedback](./insider-program.md#share-your-feedback) with us.