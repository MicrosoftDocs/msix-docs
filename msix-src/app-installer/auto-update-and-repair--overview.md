---
title: Auto-update and repair apps
description: Overview of the auto-update and repair settings for Windows apps installed using an AppInstaller file.
ms.date: 4/15/2021
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller
ms.localizationpriority: medium
---

# Auto-update and repair apps
>[!Important]
> The following article discusses settings currently available in Windows Insider build 22415 and newer.

The auto-update and repair settings allows developers and IT pros to provide an automated update solution to Windows apps that are distributed without the use of the Microsoft Store. By specifying auto-updates and repair settings as part of your App Installer file, the Windows app can be configured to check for updates on every launch, hide the updating / repairing prompt, and/or prevent the Windows app from launching until it has received the latest update.

Installing a Windows app using the App Installer file will create an entry in the App Installer repository with the specified configurations that had been set. As long as the Windows app has an entry in the App Installer repository, the automatic update and repair of the app can be configured through by: Windows Settings App, App Installer file, PowerShell, or through a CSP. Any changes that are made to any specific Windows app will overwrite all settings for that particular Windows app.

The Windows Settings App provides the ability to enable / disable the automatic update and repair of Windows apps.

> [!NOTE]
> There are instances when a setting will not be overwritten, any settings configured via CSP will over ride all other configurations, PowerShell, and the App Installer file will override any settings configured by the develop. 

## Automatic updates

Windows apps will use their App Installer URI path to check for Windows app updates, however if the App Installer URI is inaccessible the Windows app will check for updates using the UpdateURIs, attempting to connect to each before attempting the next. The first App Installer file to be accessible will be validated against checking for any new Windows app updates.

Updating of Windows apps supports the following elements:

| Elements                 | Description                                                                                                                     |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| HoursBetweenUpdateChecks | Defines the minimal gap in Windows app update checks.                                                                           |
| UpdateBlocksActivation   | Defines the experience when an app update is checked for.                                                                       |
| ShowPrompt               | Defines if a window is displayed when updates are being installed, and when updates are being checked for.                      |
| UpdateURI                | The URI to the fallback App Installer file which can be used to update the Windows app when the App Installer URI is unavailable. |

For instructions on how to create an App Installer file with the above settings, please visit the [Creating an App Installer file](how-to-create-appinstaller-file.md) Docs article.

### Embedded App Installer file

The embedded App Installer enables Windows app developers to configure the update settings for their Windows apps. The above listed settings can be set for a specific Windows app. Allowing for updates to be delivered for your Windows app from your preferred update hosting solution.

For more information on how to embed an App Installer file in your Windows app: [Using the App Installer file to update your app](how-to-embed-an-appinstaller-file.md)

### App Installer file

The App Installer file enables Windows app developers, or IT pros to configure the update settings for the Windows apps. The App Installer file will override all settings configured by an embedded App Installer file.

### PowerShell

The PowerShell cmdlets allow an IT pro to read or configure the update and repair settings of their Windows apps. 

| PowerShell Cmdlet                   | Description                                                                                                              |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `Get-AppxPackageAutoUpdateSettings` | Returns the currently set auto-update and repair settings for a specific or all configured Windows apps.                 |
| `Set-AppxPackageAutoUpdateSettings` | Configures the auto-update and repair settings for a specific Windows app that was installed using an App Installer file. |

See the Get-AppxPackageAutoUpdateSettings and Set-AppxPackageAutoUpdateSettings Docs Articles for more information on how to use these PowerShell cmdlets.

### CSP

Enterprise IT pro's make use of mobile device management solutions (such as: Microsoft Endpoint Manager) to manage their devices remotely. The Enterprise Modern App Management CSP has been expanded to include the settings which can be applied to Windows 10 devices to manage the automatic updating of specific Windows apps.

The following CSP Settings can be found in the following path: `./Device/Vendor/MSFT/EnterpriseModernAppManagement/AppManagement/nonStore/<Windows app Family Name>/AppUpdateSettings/AutoUpdateSettings/AutoUpdateSettings/`

| CSP                         | Description                                                                                   |
|-----------------------------|-----------------------------------------------------------------------------------------------|
| ./PackageSource             | Specifies the Source to the *.appinstaller file used to check for Windows app updates.        |
| ./AutomaticBackgroundTask   | Specifies whether the Windows app will check for and update the Windows app in the background |
| ./OnLaunchUpdateCheck       | Specifies if the Windows app will check for updates when launched.                            |
| ./HoursBetweenUpdateChecks  | Specifies the time between Windows app Update checks.                                         |
| ./ShowPrompt                | Specifies whether the user will be prompted with update or repair dialogs.                    |
| ./UpdateBlocksActivation    | Specifies whether the Windows app will launch if an update is available.                      |
| ./ForceUpdateFromAnyVersion | Specifies whether the Windows app update can be both up-level or down-level.                  |
| ./Disable                   | Specifies whether the Auto-Update setting is enabled / Disabled for a specific package.       |

For more information on the CSP, please visit the [Enterprise Modern App Management](/windows/client-management/mdm/enterprisemodernappmanagement-csp) CSP Docs Article.

## Automatic Repair

Windows apps will use their App Installer URI path to identify where the Windows app can source it's repair from. If the App Installer URI is inaccessible or not configured, it will then attempt to access a Windows app file from the `RepairURIs`. 

| Elements  | Description                                                                                                                     |
|-----------|---------------------------------------------------------------------------------------------------------------------------------|
| UpdateURI | The URI to the fallback App Installer file which can be used to update the Windows app when the App Installer URI is unavailable. |

For more information on how to create an *.AppInstaller file, see [How to create an App Installer file](how-to-create-appinstaller-file.md) or download and use the [App Installer File Builder](https://aka.ms/msix-toolkit) as part of the MSIX Toolkit.

### CSP

Enterprise IT pro's make use of mobile device management solutions (such as: Microsoft Endpoint Manager) to manage their devices remotely. The Enterprise Modern App Management CSP has been expanded to include the settings which can be applied to Windows 10 devices to manage the automatic repair of specific Windows apps.

The following CSP Settings can be found in the following path: `./Device/Vendor/MSFT/EnterpriseModernAppManagement/AppManagement/nonStore/<Windows app Family Name>/AppUpdateSettings/AutoUpdateSettings/AutoRepair/`

| CSP             | Description                                                                                           |
|-----------------|-------------------------------------------------------------------------------------------------------|
| ./PackageSource | Specifies the Source to the *.appinstaller or Windows app file used to check for Windows app repairs. |

For more information on the CSP, please visit the [Enterprise Modern App Management](/windows/client-management/mdm/enterprisemodernappmanagement-csp) CSP Docs Article.
