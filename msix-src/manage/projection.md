---
description: This guide the ability to project files 
title: Ability to project files in MSIX packages
ms.date: 03/17/2021
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Ability to project files in MSIX packages
In order to satisfy existing ecosystem requirements, apps may require the files to appear to exist in their existing install directory. For example, if a particular app was expecting a file in a folder, like C:\Program Files\Contoso; that directory can be modified by the admins. So starting in Windows 11, apps can specify a directory outside of the WindowsApps directory, and deployment will use filter drivers in order to make the files appear in that location with the ACLs inherited from the parent directory. 





