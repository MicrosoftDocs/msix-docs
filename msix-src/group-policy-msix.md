---
title: How Group Policy work with MSIX packaged apps
description: Describes how Group Policy works with apps Converted to MSIX
author: dianmsft
ms.author: diahar
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---
## How Group Policy works with MSIX packaged apps
For developers, with MSIX you can leverage group policy like you would any other installer types. If you have packaged your Win32 app into an MSIX (or if you built your app using the desktop bridge), your app has the full trust capability enabled. This allows you to read from the group policy registry keys. The app at run time will have the same view of the group policy registry as it would any other app. If the app is a Universal Windows Platform (UWP) app starting in Windows 10 1809, these apps can access the same group policy keys. 
More information on creating group policy can be found [here]( https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

For IT Pros who are converting existing installers to MSIX, there is no new work needed for these apps to support group policy. Continue to manage group policies as you normally would for the original installer. Converted apps to MSIX will still be able to read from existing group policy registry keys. 
More information on converting existing installer to MSIX can be found [here]( https://docs.microsoft.com/en-us/windows/msix/mpt-overview).
