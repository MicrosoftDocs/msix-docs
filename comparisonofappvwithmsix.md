---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       # Add a title for the browser tab
description: # Add a meaningful description for search results
author:      fiza-microsoft # GitHub alias
ms.author:   fizaazmi # Microsoft alias
ms.prod:  # Add the
ms.topic:    # Add the ms.topic value
ms.date:     03/27/2023
---

# App-V vs MSIX

  
## Description  
  
This article contains a side-by-side comparison of App-V and MSIX.  
Since Application Virtualization will be [end of life in April 2026](/lifecycle/announcements/mdop-extended), we recommend looking at Azure Virtual Desktop with MSIX App Attach.  
  
  
## Feature-wise comparison

|Feature|App-V|MSIX|
| -------- | -------- | -------- |
|Sharing across apps |A connection group is an App-V feature that can group packages together to create a virtual environment where applications within that package group can interact with each other. |Shared package containers allows IT Pros to create a shared runtime container for packaged application – sharing a merged view of the virtual file system and virtual registry - enabling access to one another’s package root files and state |
|Development focus |Application Virtualization 5.1 for Remote Desktop Services will be end of life on January 10, 2023. |Relatively newer, introduced in 2018.|
|How updates are handled |While App-V does divide app into 64k blocks, the update needs the complete app to be downloaded|Only the delta (differential updates) are downloaded |
|A single copy of a file across apps/users |File duplication across apps is not avoided |A single copy of any file is kept.  |
|Signing |Does not seem required |Required|

  
  
## For more information
  
[What is Azure Virtual Desktop?](/azure/virtual-desktop/overview)  
[Set up MSIX app attach with the Azure portal](/azure/virtual-desktop/app-attach-azure-portal)


