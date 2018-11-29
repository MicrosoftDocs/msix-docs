---
title: AppInstaller file updates | Microsoft Docs
description: Learn how to update the AppInstaller file
author: laurenhughes
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Update Settings

As mentioned in [AppInstaller file overview](app-installer-file-overview.md), you can select what kind of update behavior the app should have. Here we explore the update options and their respective trade-offs.

In short, you can choose to check for updates 2 different ways: 1) completely independently of the user launching the app 2) only when the user launches the app; 
Additionally, you can choose to apply updates in 2 different ways: 1) by informing the user with a prompt 2) silently, without informing the user.
Finally, when you inform the user of an update, you can either force them to take the update before allowing them to launch the app, or you can allow them to launch the app and apply the update at an opportune time. 

Specifically, the syntax that is available to you is the following: 

- Update settings “UpdateSettings” can have the following elements: 

    - “OnLaunch”, which checks for updates on launch. This type of update can show UI and has the following attributes:

        - “ShowPrompt” – a Boolean that determines if UI will be shown to the user

        - “UpdateBlocksActivation” – a Boolean that determines if the UI shown to the user allows the user to launch the app without taking the update, or if the user must take the update before launching the app. This attribute can be set to “true” only if “ShowPrompt” is set to “true”. UpdateBlocksActivation =“true” means the UI the user will see allows the user to take the update or close the app. UpdateBlocksActivation =“false” means the UI the user will see allows the user to take the update or start the app without updating. In the latter case, the update will be applied silently the next time the app is launched.

        - “HoursBetweenUpdateChecks” – an integer that indicates how often (in how many hours) the system will check for updates to the app. “0” to “255” inclusive. The default value is 24 (if this value is not specified). For example if HoursBetweenUpdateChecks = 3 then when the user launches the app, if the system has not checked for updates within the past 3 hours, it will check for updates now.  
    
    - “AutomaticBackgroundTask” –  which checks for updates in the background every 8 hours independently of whether the user launched the app. This type of update cannot show UI.
    
    - “ForceUpdateFromAnyVersion” –  which allows the app to update from version x to version x++ or to downgrade from version x to version x--. Without this element, the app can only move to a higher version.
    

