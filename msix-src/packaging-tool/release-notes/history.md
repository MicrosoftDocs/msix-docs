---
title: MSIX Packaging Tool release notes
description: Full history of MSIX Packaging Tool release notes
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# MSIX Packaging Tool Release Notes

### Version 1.2019.919.0
- Device Guard signing is now available. This signing option requires an active Microsoft Azure Active Directory account configured for the Microsoft Store for Business. For more information, see [this article](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing).
- The **Package editor** now supports the ability to select multiple items on which to perform an action.

### Version 1.2019.808.0
- Settings improvements
    - Separated out the tool defaults from other settings
    - Added the ability to import and export settings
- Signing improvements
    - You can now choose a default signing option for workflows
- Updated the steps during packaging to improve the experience

## Version 1.2019.701.0 - Public Release

- Support for desktop installers that require restart - [learn more](../support-restart.md)
    - Auto-login option for restart 
- New options in app settings
    - Specify a default cert to sign packages with 
    - Specify exit codes for installers that require restart
    - Exit code for restarts list in Settings includes common default values
- Ability to add new, unknown, and custom capabilities through the package editor
- Automatically sets MinVersion to 1709 when Store versioning requirements are turned off in Settings
- New folders can be added under Assets in Package editor
- Restore default settings and exclusion items now also clears signing certificate password and exit codes
- Fixed an issue where first launch tasks weren't getting properly deleted
- Will ignore shortcuts to excluded files during package creation
- Defaults to signing a package if a default signing certificate is specified in the settings
- Honor PowerShell installer exit codes
- Informs the user when they need a restart for the driver
