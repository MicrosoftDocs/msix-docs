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

 > [!NOTE] 
 > Device restart is a new feature that is currently only available in the insider preview build.
 > You can join the MSIX Packaging Tool Insider Preview Program to get access to this feature.

We now support the ability to restart during your conversion process using the MSIX Packaging Tool! 

Restarts are supported both through our interactive UI and through our Command Line Interface. In addition, through our Command Line, you can also specify you want to auto-login after the restart to have a seamless hands-free conversion. 

You can specify which exit codes indicate a restart in the **Settings (Installer exit codes) page**. 

You can trigger a restart through the UI during your conversion workflow or through the start menu of your conversion machine. After the restart has happened, you will return to the same place you were in the tool to continue with your conversion.

Please keep in mind this feature is still in development and there may be some bugs. If you find any, be sure to [file feedback](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/insider-program#share-your-feedback) so we can fix it!

