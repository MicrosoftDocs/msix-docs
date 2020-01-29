---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing your MSIX deployment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Creating your MSIX
## Visual Studio
MSIX has been supported natively in [Visual Studio](https://visualstudio.microsoft.com/downloads/) since version Visual Studio 2017. MSIX is the modern way to deploy applications on Windows. It’s built to be safe, secure and reliable, and lets you and your customers take advantage of the new app model and the modern APIs that have been introduced in Windows 10, regardless of whether you intend to upload your apps to the Microsoft Store or sideload them onto computers in your enterprise.  

Most developers using [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) today will publish their installer using MSIX.  Creating a MSIX can be as simple as choosing Publish from the Visual Studio menu. For a deep dive into building MSIX file in Visual Studio as well as integrating this into your CI / CL build environment, see [MSIX: The Modern Way to Deploy Desktop Apps on Windows.](https://docs.microsoft.com/archive/msdn-magazine/2019/june/devops-msix-the-modern-way-to-deploy-desktop-apps-on-windows)

## Converting your installer
For those that are not writing new code, but instead deploying existing applications, the tool to help you do this is the [MSIX Packaging Tool.](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab)   With the use of this tool, you can convert your existing MSI or installer to MSIX for installation on supporting operating systems.  For details on the MSIX Packaging Tool, see [MSIX Packaging Tool.](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-overview)

## 	Bulk Operations 
Often developers need to convert more than one package and deploy it.  The [MSIX Toolkit](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) provides easy to use powershell scripts for batch conversion of your applications to MSIX. 

