---
title:       Feature-based comparison of Application Virtualization and MSIX
description: side-by-side comparison of App Virtualization (App-V) and MSIX 
author:      v-fvalentyna
ms.author:   v-fvalentyna
ms.topic:    product-comparison
ms.date:     10/05/2026
---

# Feature-based comparison of Application Virtualization and MSIX

While mainstream support for Application Virtualization (App-V) has ended, fixed extended support for App-V Server continues through October 9, 2029 (see [App-V in Windows support policy](/microsoft-desktop-optimization-pack/app-v/appv-support-policy)). As an alternative, consider *Azure Virtual Desktop with MSIX App Attach*. This article contains a side-by-side comparison of App Virtualization (App-V) and MSIX.

## Compare the features of App-V and MSIX

This table compares the basic features of App-V with MSIX:

|Feature|App-V|MSIX|
| -------- | -------- | -------- |
|Sharing across apps |A connection group feature in App-V groups packages together to create a virtual environment. This feature allows applications within that package group to interact with each other. |Shared package containers in MSIX allow you to create a shared runtime container for packaged applications. This feature enables sharing a merged view of the virtual file system and virtual registry, thus providing access to one another's package root files and state.|
|How updates take place |App-V divides the app into 64-KB blocks. However, you need to download the complete app to get updates. |Only the delta (differential) updates are downloaded. |
|A single copy of a file across apps/users |File duplication across apps can happen. |Keeps a single copy of any file. |
|Signing |Not required|Required|

## For more info

Learn more about Azure Virtual Desktop at [What is Azure Virtual Desktop?](/azure/virtual-desktop/overview).

Learn more about MSIX app attach at [What is MSIX app attach?](/azure/virtual-desktop/what-is-app-attach).
