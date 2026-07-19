---
title: App Installer file update settings
description: This article describes options for how to configure the behavior of app updates by using the App Installer file.
ms.date: 07/02/2026
ms.topic: how-to
keywords: windows 10, uwp, msix
ms.custom: RS5
---

# Configure update settings in the App Installer file

As mentioned in [App Installer file overview](app-installer-file-overview.md), you can configure the update behavior of the app in the App Installer file. This article explores the update options and their respective trade-offs.

You can configure the update behavior of the app by using the [UpdateSettings](/uwp/schemas/appinstallerschema/element-update-settings) element. Here we explore the update options and their respective trade-offs.

In short, you can choose to check for updates two different ways:
1. Independently of the user launching the app.
2. Only when the user launches the app.

Additionally, you can choose to apply updates in two different ways:
1. By informing the user with a prompt.
2. Silently, without informing the user.

Finally, when you inform the user of an update, you can either force them to take the update before allowing them to launch the app, or you can allow them to launch the app and apply the update at an opportune time.


The [UpdateSettings](/uwp/schemas/appinstallerschema/element-update-settings) element can have the following child elements:

| App Installer file update setting | Min Windows 10 Version
|------------------|--------------------|
|  OnLaunch| 1709                |
|  HoursBetweenUpdateChecks| 1803                |
| AutomaticBackgroundTask | 1803 |
| UpdateBlocksActivation  | 1903 |
|  ShowPrompt | 1903 |
|  ForceUpdateFromAnyVersion | 1903 |

- **OnLaunch**: Checks for updates on launch. This type of update can show UI and has the following attributes:

    > [!IMPORTANT]
    > **OnLaunch** update checks happen only when the app is launched from the **Start menu** (including Start menu tiles). By design, they are **not** triggered when the app is launched from a **desktop shortcut** or a **taskbar** item&mdash;those launches start the currently installed version without checking for updates. This behavior reduces friction for task-focused launches. If you need updates to be checked regardless of how the app is launched, check for and apply updates from your app's code. For more information, see [Update non-Store published apps from your code](../non-store-developer-updates.md).

    - **HoursBetweenUpdateChecks**: An integer from “0” to “255”, inclusive, that sets the minimum number of hours between update checks. The default value is 24 (if this value is not specified). On launch, the system checks for updates only if at least this many hours have passed since the last check. For example, if HoursBetweenUpdateChecks = 3, then when the user launches the app, the system checks for updates only if it hasn't checked within the past 3 hours.

     - **ShowPrompt**: A boolean that determines if UI will be shown to the user. This value is supported on Windows 10, version 1903 and later.

     - **UpdateBlocksActivation**: A boolean that determines if the UI shown to the user allows the user to launch the app without taking the update, or if the user must take the update before launching the app. This attribute can be set to “true” only if **ShowPrompt** is set to “true”. **UpdateBlocksActivation**=“true” means the UI the user will see, allows the user to take the update or close the app. **UpdateBlocksActivation**="false" means the UI the user will see, allows the user to take the update or start the app without updating. In the latter case, the update will be applied silently at an opportune time. This value is supported on Windows 10, version 1903 and later.

        > [!NOTE]
        > ShowPrompt needs to be set to true if UpdateBlocksActivation is set to true.

- **AutomaticBackgroundTask**: Checks for updates in the background every 8 hours independently of whether the user launched the app. This type of update cannot show UI.

- **ForceUpdateFromAnyVersion**: Allows the app to update from version x to version x++ or to downgrade from version x to version x--. Without this element, the app can only move to a higher version.
