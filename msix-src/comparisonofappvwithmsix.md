---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       Feature-based comparison of Application Virtualization and MSIX
description: side-by-side comparison of App Virtualization (App-V) and MSIX 
author:      fiza-microsoft # GitHub alias
ms.author:   fizaazmi # Microsoft alias
ms.topic:    product-comparison
ms.date:     04/05/2023
---

# Feature-based comparison of Application Virtualization and MSIX

Application Virtualization (App-V) will be [end of life in April 2026](/lifecycle/announcements/mdop-extended). We recommend looking at Azure Virtual Desktop with MSIX App Attach as an alternative. This article contains a side-by-side comparison of App Virtualization (App-V) and MSIX.

## Compare the features of App-V and MSIX

  
The following table compares the basic features of App-V with MSIX:
|Feature|App-V|MSIX|
| -------- | -------- | -------- |
|Sharing across apps |A connection group feature in App-V groups packages together to create a virtual environment. This allows applications within that package group to interact with each other.|Shared package containers in MSIX allow IT Pros to create a shared runtime container for packaged application. This enables sharing a merged view of the virtual file system and virtual registry, thus providing access to one another’s package root files and state. |
|Development focus |End of life is April 2026.|Relatively newer, introduced in 2018.|
|How updates are handled |App-V divides app into 64k blocks; however, the complete app needs to be downloaded to get the updates.|Only the delta (differential updates) are downloaded. |
|A single copy of a file across apps/users |File duplication across apps is not avoided |A single copy of any file is kept|
|Signing |Not required|Required|

## For more information

Learn more about Azure Virtual Desktop at [What is Azure Virtual Desktop?](/azure/virtual-desktop/overview)  
Learn more about MSIX app attach at [What is MSIX app attach?](/azure/virtual-desktop/what-is-app-attach)

