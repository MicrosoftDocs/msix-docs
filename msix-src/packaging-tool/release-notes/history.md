---
title: MSIX Packaging Tool release notes
description: Full history of MSIX Packaging Tool release notes
author: c-don
ms.author: cdon
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# MSIX Packaging Tool Release Notes

### Version 1.2019.621.0
- Exit code for restarts list in settings includes common default values
- Ability to add new, unknown, and custom capabilities through the package editor manifest editor

### Version 1.2019.618.0

- Automatically sets MinVersion to 1709 when Store versioning requirements are turned off in Settings
- New folders can be added under Assets in Package editor

### Version 1.2019.611.0

- Restore default settings and exclusion items now also clears signing certificate password and exit codes
- Fixed an issue where first launch tasks weren't getting properly deleted
- Will ignore shortcuts to excluded files during package creation

### Version 1.2019.604.0

- Defaults to signing a package if a default signing certificate is specified in the settings
- Allow negative installer codes to be specified in the settings
- Honor PowerShell installer exit codes
- Informs the user when they need a restart for the driver

### Version 1.2019.522.0

New Features:

- Support for desktop installers that require restart - [learn more](../support-restart.md)
    - Auto-login option for restart 
- New options in app settings
    - Specify a default cert to sign packages with 
    - Specify exit codes for installers that require restart
    
Known Issues:

- Negative reboot exit codes are currently not supported
- If Default cert is specified, each conversion workflow will need to select 'use cert'
- During remote or VM restarts, there might be an extra login prompt 
- Restore defaults button doesn't remove certificate password or installer exit codes

## Version 1.2019.402.0 - Public Release

 - Ability to convert on a remote machine - [more info](../remote-conversion-setup.md)
 - Validate COM ProgId type values, COM class entries and remove invalid COM registrations
 - Update Windows SDK tools that are redistributed in MSIX Packaging Tool 
 - Automatically split arguments into a separate fields for manifested com executables
 - Robust handling of PowerShell installer arguments
 - Made disabling Windows Update service a required step in the machine prep phase
- Added the ability to time stamp your signed package in all of the workflows where signing is currently available
    - You can specify your default time stamp URL and type of time stamp server in the tool Settings page 
- Empty folders created in the VFS of the Package Editor will persist after saving the package
- User can specify valid expected exit codes for CLI conversions
- Empty folders generated during a conversion will persist through packaging
- Updated AppID generation logic, and added additional validation for package name and app 
- Improved management experience in package editor
- Auto versioning recommendations when saving in package editor
- Added the ability to use “.” to progress the version field
- Fixed validation for installation location
- Fixed manifest translation issues for file type association handlers and com server entries
- Added the ability to get the status of your command line conversions
- Improved COM warning logging with human readable errors
