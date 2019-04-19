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

# Group policy and MSIX packaged apps

Developers who use MSIX can leverage group policy similar to any other installer types.

If you have packaged your Win32 app into an MSIX (or if you built your app using the Desktop Bridge), your app has the full trust capability enabled. This allows you to read from the group policy registry keys. At run time, your app will have the same view of the group policy registry as it would any other app. If your app is a Universal Windows Platform (UWP) app, starting in Windows 10 1809 your app can access the same group policy keys. For more information about creating group policy, see [this article](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

If you are converting an existing installer to MSIX by using the [MSIX Packaging Tool](mpt-overview.md), there is no new work needed for your app to support group policy. Continue to manage group policies as you normally would for the original installer. Apps converted to MSIX will still be able to read from existing group policy registry keys.