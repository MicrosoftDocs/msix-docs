---
title: MSIX Packaging Tool release notes
description: This article provides the full history of release notes for different versions of the MSIX Packaging Tool.
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# Release notes for the MSIX Packaging Tool
## Version 1.2020.225.0
- Ability to import registry files (.reg) in Package Editor
- Ability to automatically add [MSIX Core support](../msix-core/msixcore.md) to an MSIX
- Add InstalledLocationVirtualization extension
Fixed bugs:
- Validate manifest format after editing 
- Make message when First Launch Task fails clearer 
- Forbid relative paths for Unpack 
- Updated file filters so they show what formats are valid (e.g., for installers it used to say *.* and now `*.msi, *.exe, ...`) 
- Fixed when Unpack would convert spaces in paths into "%20"

## Version 1.2020.203.0
- Package Integrity setting is on by default
- Create package page now shows the default save location
- Ability to export your registry files(.reg)
- General bug fix improvements

## Version 1.2019.1220.0 - Public Release
- Support for converting an existing installer with services is now available
  - See all detected services and modify them in the new [Services report page](../convert-an-installer-with-services.md)
- New setting to specify the default conversion environment
- New setting to specify the default installer browse location
- New setting to add Package Integrity to apps
- Add run and remove buttons to first launch tasks page
- Added the ability to drag and drop files to move them in package editor
- Added fix for an issue with translating COM server paths that was causing them to be invalid
