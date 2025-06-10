---
title:       Feature-based comparison of Application Virtualization and MSIX
description: side-by-side comparison of App Virtualization (App-V) and MSIX 
author:      fiza-azmi
ms.author:   fizaazmi
ms.topic:    product-comparison
ms.date:     04/05/2023
---

# Feature-based comparison of Application Virtualization and MSIX

Application Virtualization (App-V) will be end-of-life in April 2026 (see [Microsoft Desktop Optimization Pack (MDOP) support extended](/lifecycle/announcements/mdop-extended)). As an alternative, we recommend that you look at *Azure Virtual Desktop with MSIX App Attach*. This article contains a side-by-side comparison of App Virtualization (App-V) and MSIX.

## Compare the features of App-V and MSIX

This table compares the basic features of App-V with MSIX:

|Feature|App-V|MSIX|
| -------- | -------- | -------- |
|Sharing across apps |A connection group feature in App-V groups packages together to create a virtual environment. This allows applications within that package group to interact with each other.|Shared package containers in MSIX allow IT Pros to create a shared runtime container for packaged application. This enables sharing a merged view of the virtual file system and virtual registry, thus providing access to one another's package root files and state. |
|Development focus |End-of-life is April 2026.|Relatively newer; introduced in 2018.|
|How updates are handled |App-V divides the app into 64KB blocks; however, the complete app needs to be downloaded in order to get the updates.|Only the delta (differential updates) are downloaded. |
|A single copy of a file across apps/users |File duplication across apps isn't avoided. |A single copy of any file is kept.|
|Signing |Not required|Required|

## For more info

Learn more about Azure Virtual Desktop at [What is Azure Virtual Desktop?](/azure/virtual-desktop/overview).

Learn more about MSIX app attach at [What is MSIX app attach?](/azure/virtual-desktop/what-is-app-attach).
