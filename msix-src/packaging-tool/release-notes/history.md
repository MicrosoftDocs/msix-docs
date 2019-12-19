---
title: MSIX Packaging Tool release notes
description: This article provides the full history of release notes for different versions of the MSIX Packaging Tool.
ms.date: 03/05/2019
ms.topic: article
keywords: windows 10, uwp, msix, msix packaging tool, insider program
ms.localizationpriority: medium
ms.custom: Vibranium
---

# Release notes for the MSIX Packaging Tool

## Version 1.2019.1218.0
- Added the ability to drag and drop files to move them in package editor
- New setting to add Package Integrity to apps
- Show default save location on the create package page

## Version 1.2019.1028.0 
- Support for converting an existing installer with services is now available
	- See all detected services and modify them in the new [Services Report page](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/Converting-an-installer-with-services)
- New setting to specify the default conversion environment
- New setting to specify the default installer browse location
- Add run and remove buttons to first launch tasks page

## Version 1.2019.1018.0 - Public Release

- Updated strings for public release
- Device Guard signing is now available. This signing option requires an active Microsoft Azure Active Directory account configured for the Microsoft Store for Business. For more information, see [this article](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing).
- The **Package editor** now supports the ability to select multiple items on which to perform an action.
- Right-click to edit an MSIX package is now supported.
- Ux improvements for the packaging workflow.
- Settings improvements:
    - Separated the tool defaults from other settings.
    - Added the ability to import and export settings.
- Signing improvements:
    - You can now choose a default signing option for workflows.
- Updated the steps during packaging to improve the experience.

