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

## Version 1.2021.422.0
- Improved warnings about timestamping for the Device Guard Signing version 2 option
- Fixed an issue where settings was verifying signing each time settings was opened
- Shortcut detection improvements
- Added more default exclusion paths to improve logs
- General bug fixes

## Version 1.2020.1219.0 - Public Version
- Removed Device Guard Signing version 1 support. See documentation on how to use [version 2](../../package/signing-package-device-guard-signing.md).If you have any questions, contact the Device Guard Signing Support Team DGSSMigration@microsoft.com
- Fixed an issue where clicking in the same line in package files didn't work to highlight the item
- UX improvements for select installer page
  - Allow installers in PATH to be specified witout giving the full path
  - Show the command line string used to execute the installer selected
  - Extend the installer arguments text box for long argument strings so that they are easier to review and edit
- Only add a default logo for the store if one is not extracted from the application
- General bug fixes
