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

## Version 1.2020.824.0
- Added a Services Report to the Package Editor
- General bug fixes

## Version 1.2020.709.0 - Public Release
- Ability to add multiple files to the package editor
- Ability to import multiple .reg files to the package editor
- Improved support for converting any installer type
- Ability to add items from the package editor to the exclusion list
- Use ctrl to multi-select options in package editor
- Added a prompt when overwriting files when moving or adding
Fixed bugs:
- Disable selecting local vm machine when Hyper-V is not present
- When enforce store requirements checkbox is selected, prevent creation of icons not accepted by store
- Fixed an issue with App-V registry values during conversion
- Added a longer timeout session for remote command line conversions
- Improved MSIX Core OS selections to reduce conflicts and confusion

