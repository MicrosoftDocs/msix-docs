---
title: Best Practices for MSIX Packaging Tool
description: Best Practices for MSIX Packaging Tool 
author: lahugh
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
---

# Best practices for MSIX Packaging tool

This article covers a list of best practices for repackaging your app to MSIX and using the MSIX Packaging Tool. 

To begin with, to have support of MSIX, you need to be on a RS5 build of the MSIX Packaging Tool.

- For the conversion process, there are a few other things that we recommend you consider before you start. 

- First, we understand that not everyone is on RS5 or even Windows 10. So we recommend that you create a clean VM that is pre-configured for the min version of support for MSIX. Another reason this is a recommendation is that during the interactive GUI conversion using the MSIX Packaging Tool, we will be listening to everything on the device, and it will help to prevent extraneous data in your package. 

- Its also good to know what kind of dependencies you have so that you can understand which ones you should run with your app and which should be packaged as a modification package. For example, if you have runtime dependencies, it’s a good idea to include those in your main application. If you have a plug in, you should package that as an associated modification package. 

---

When you are going through the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice.

- When Packaging ClickOnce installers it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.

- When creating modification packages, you need to declare the Package Name (Identity Name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.

- Declaring an installation location field in Package information page is optional but recommended. Make sure that this path matches the installation location of application Installer.

- Performing the preparation steps in Prepare Computer page is optional but highly recommended.



