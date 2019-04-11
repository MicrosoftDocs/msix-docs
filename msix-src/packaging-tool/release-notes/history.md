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

#### Ver 1.2019.402.0

 - Validate COM ProgId type values, COM class entries and remove invalid COM registrations
 - Update Windows SDK tools that are redistributed in MSIX Packaging Tool 

#### Ver 1.2019.322.0

 - Automatically split arguments into a separate fields for manifested com executables

#### Ver 1.2019.315.0

 - Robust handling of PowerShell installer arguments
 - Made disabling Windows Update service a required step in the machine prep phase

#### Ver 1.2019.308.0

- Added the ability to time stamp your signed package in all of the workflows where signing is currently available
    - You can specify your default time stamp URL and type of time stamp server in the tool Settings page 
- Empty folders created in the VFS of the Package Editor will persist after saving the package

#### Ver 1.2019.304.0

New Features:

- User can specify valid expected exit codes for CLI conversions
- Empty folders generated during a conversion will persist through packaging
- Updated [AppID generation logic](#appid-generation-logic), and added additional validation for package name and app 

##### AppID generation logic
The current procedure to derive the App ID is as follows: 
1. Find the exe/msi name, and strip the extension
2. Convert to uppercase
3. Remove all non alpha-numeric characters
4. Translate the numerals to corresponding English words
5. If the resulting string contains less than 3 characters, use the string “APP” instead
6. If the resulting string contains more than 64 characters, truncate it.
7. For collisions, truncate the string if it contains more than 62 characters, and append a two-digit value, starting with 00.

#### Ver 1.2019.226.0
New Features:

- Ability to convert on a remote machine - [more info](../remote-conversion-setup.md)
- Improved management experience in package editor
- Auto versioning recommendations when saving in package editor

Additional updates

- Added the ability to use “.” to progress the version field
- Fixed validation for installation location
- Fixed manifest translation issues for file type association handlers and com server entries
- Added the ability to get the status of your command line conversions
- Improved COM warning logging with human readable errors

 #### Ver 1.2019.110.0
:point_up_2: **Public Release**

 > [!Public Release]

New Features:

- Improved packaging times 
- Updated default file exclusion list
- Incorporated MSIExec error logs into tool reporting
- Updated logs to add more clarity and troubleshooting steps
- Added support for capturing installation from PowerShell ISE during manual packaging
- Added support for declaring PowerShell scripts as installer argument in the UI and the command line template file
- Added a verbose logging flag(--verbose | -v) for the Command line interface
- Fixed an issue where network paths on the VM were sometimes inaccessible
- Fixed an issue where Store versioning requirement validation was failing when using the command line interface
- Fixed an issue where file paths in quotations were not being accepted
- Fixed an issue where the VM was not being cleaned up correctly after conversion
- Fixed an issue where adding files to packages in package editor was not working properly
- UI cleanup 