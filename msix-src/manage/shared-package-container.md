---
Description: This guide explains MSIX Shared Package Container
title: MSIX Shared Package Container
ms.date: 03/17/2021
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# MSIX Shared Package Container 
Shared package container allows IT Pros to create a shared runtime container for MSIX packaged application – sharing a merged view of the virtual file system and virtual registry - enabling access to one another’s package root files and state. The ability to allow IT Pros to manage what apps can be in what container is important to the conversion of MSIX from legacy installers. The concept of a shared container is used primarily for customization, sharing pre-requisite software, and supporting addons for converted apps. Please note that this is an enterprise only feature and will require administrative privileges to use.  

Shared Package Container operations are independent of app deployment operations. What this means is that apps do not have to be installed prior to share package container definition being deployed to a device. It also means that not all apps that are defined inside the shared package container need to be installed for the shared package container to run. The apps inside the shared package container will be able to independently update without having to modify the shared package container definition.  

Note that an app will only be allowed to be inside one container. Deploying a shared package container that contains an app that is already part of a shared package container will result in an error.  

