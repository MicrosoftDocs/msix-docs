---
title: How Group Policy works with MSIX packaged apps
description: Describes how Group Policy works with apps that are converted to MSIX.
ms.date: 04/12/2019
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Group Policy and MSIX packaged apps

Developers who use MSIX can leverage Group Policy in a similar way to other installer types.

If you have packaged your Win32 app into an MSIX (or if you built your app using the Desktop Bridge), your app has the full trust capability enabled. This allows you to read from the Group Policy Registry keys. At run time, your app will have the same view of the Group Policy Registry as it would if it had been installed using a different method. Starting in Windows 10 1809, if your app is a Universal Windows Platform (UWP) app it can access the same Group Policy keys. For more information about creating Group Policy, see [this article](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

If you are converting an existing installer to MSIX by using the [MSIX Packaging Tool](mpt-overview.md), there is no new work needed for your app to support Group Policy. Continue to manage Group Policies as you normally would for the original installer. Apps converted to MSIX will still be able to read from existing Group Policy Registry keys. 

Group Policy does not have native support to install MSIX applications. 
