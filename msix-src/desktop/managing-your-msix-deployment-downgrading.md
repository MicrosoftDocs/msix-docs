---
Description: This article provides details on how MSIX applications handle being downgraded.
title: Install earlier versions of an MSIX app package
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
---

# Install earlier versions of an MSIX app package

Installation of earlier versions of an MSIX app package can be performed on a device without prior removal of the application. Triggering an installation of an earlier version of the same application will cause the client to review the app's manifest file contained within the installing MSIX app package. The review of the appxmanifest.xml file will identify delta differences between the currently installed app and the installer being executed on the device. This process maintains the data stored within the *appdata* folder. Any changes that were made when installing the upgraded version will not be undone if an earlier version of the MSIX app package is installed.
