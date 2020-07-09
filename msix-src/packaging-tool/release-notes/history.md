---
title: MSIX Packaging Tool release notes
description: This article provides the full history of release notes for different versions of the MSIX Packaging Tool.
ms.date: 03/25/2020
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# Release notes for the MSIX Packaging Tool

## Version 1.2020.625.0
- Bug fixes for final release version

## Version 1.2020.618.0
- Added a longer timeout session for remote command line conversions
- Improved MSIX Core OS selections to reduce conflicts and confusion

## Version 1.2020.603.0
- Fixed an issue with App-V registry values during conversion

## Version 1.2020.528.0
- Ability to add multiple files to the package editor
- Ability to import multiple .reg files to the package editor
- Improved support for converting any installer type
- Bug fixes:
	- Disable selecting local vm machine when Hyper-V is not present
	- When enforce store requirements checkbox is selected, prevent creation of icons not accepted by store

## Version 1.2020.423.0
- Ability to add items from the package editor to the exclusion list
- Use ctrl to multi-select options in package editor
- Added a prompt when overwriting files when moving or adding

## Version 1.2020.402.0 - Public Release
- Package Integrity setting is on by default
- Ability to automatically add [MSIX Core support](../../msix-core/msixcore.md) to an MSIX
- Ability to import or export registry files (.reg) in Package Editor
- Create package page now shows the default save location
- Add InstalledLocationVirtualization extension
- Improved quality of icons extracted
- Improved icon extraction from shortcuts
Fixed bugs:
- Validate manifest format after editing 
- Make message when First Launch Task fails clearer 
- Forbid relative paths for Unpack 
- Updated file filters so they show what formats are valid (e.g., for installers it used to say *.* and now `*.msi, *.exe, ...`) 
- Fixed when Unpack would convert spaces in paths into "%20"
- general bug fixes

## Version 1.2019.1220.0 - Public Release
- Support for converting an existing installer with services is now available
  - See all detected services and modify them in the new [Services report page](../convert-an-installer-with-services.md)
- New setting to specify the default conversion environment
- New setting to specify the default installer browse location
- New setting to add Package Integrity to apps
- Add run and remove buttons to first launch tasks page
- Added the ability to drag and drop files to move them in package editor
- Added fix for an issue with translating COM server paths that was causing them to be invalid
