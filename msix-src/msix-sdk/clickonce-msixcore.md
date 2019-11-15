---
title: Create a ClickOnce Solution with MSIX Core
description: This article provides a step by step instruction on how to leverage the MSIX Core bootstrapper, which creates an application using ClickOnce that will allow your users to just download a setup.exe and install their MSIX app through the MSIX Core Installer.
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Create a ClickOnce Solution MSIX Core
For those of you who have existing Win32 apps and want to support your customers that have earlier versions of Windows, you can leverage MSIXCore boostrapper which will create an application using ClikcOnce. This will allow your users to download a setup.exe and install the MSIX app through the MSIXCore Installer. 

# Set up - Hosting app on a web server
To get your app ready for bootstrapping with the MSIX Core installer, you’ll need to host your app package on a web server. This article will help you with the specifics of setting up a web app on Azure, Internet Information Services (IIS), or Amazon Web Services (AWS). This article will go through these three seperate options. 
1. Azure 
2. Internet Information Services 
3. Amazon Web Services (AWS)

## Azure 
### Prerequisite
You will need an Azure subscription. To obtain one, visit the [Azure account page](https://azure.microsoft.com/en-us/free/)

