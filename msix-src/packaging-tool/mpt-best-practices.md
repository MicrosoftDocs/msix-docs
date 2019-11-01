---
title: Best practices for the MSIX Packaging Tool
description: This article covers best practices for repackaging your app to MSIX and using the MSIX Packaging Tool.
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Best practices for the MSIX Packaging Tool

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

This article covers best practices for repackaging your app to MSIX and using the MSIX Packaging Tool.

## Best practices during setup
 
To begin with, you make sure you have the [latest version of the MSIX Packaging Tool](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/mpt-overview#latest-public-version---1201910180). For the conversion process, there are a few other things that we recommend you consider before you start. 

- The minimum OS version requirement for the MSIX Packaging Tool is Windows 10 1809. We understand that not everyone is on the Windows 10 October 2018 Update or even Windows 10. Therefore, we recommend that you create a clean VM that is pre-configured for the min version of support for MSIX. If this isn't something you have we also offer a Quick Create VM, the [MSIX Packaging Tool Environment](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/quick-create-vm) in Hyper-V, which is ready to go for conversion. 

- Another reason this is a recommendation is that during the interactive GUI conversion using the MSIX Packaging Tool, we will be listening to everything on the device, and it will help to prevent extraneous data in your package. 

- Its also good to know what kind of dependencies you have so that you can understand which ones you should run with your app and which should be packaged as a modification package. For example, if you have runtime dependencies, it’s a good idea to include those in your main application. If you have a plug in, you should package that as a modification package to associate with your main application. 


## Best practices during repackaging 
When you are using the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice:
- When packaging ClickOnce installers, it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.
- When creating modification packages, you need to declare the package Name (identity name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.
- Performing the preparation steps in the **Prepare computer** page is optional but highly recommended, as this will help reduce any extraneous data in your package. 
- It is required that you sign your package in order to install it, but we also recommend that you timestamp your certificate so that your application can be installed, even if your certificate expires. 
- Declaring an installation location field in the **Package information** page is optional. Make sure that this path matches the installation location of application installer.
ms.custom: RS5


## Best practices while bundling MSIX packages

We recommend that you bundle MSIX packages when there are different installers available for different processor architectures such as x86, x64, and ARM for your application. By bundling the installers together, you can allow the Windows 10 OS to determine the applicability of the appropriate installer instead of the user having to make the correct selection. 

While bundling MSIX packages, you will need to have already converted your Win32 installers into MSIX packages. 

- The MSIX Packaging Tool will assume the processor architecture of the Windows 10 OS version that the conversion is taking place. For example, if you are on a x64 version of Windows 10, and you are converting a x86 version of an installers, the resulting MSIX package will be x64. 
- If you plan to bundle MSIX packages, you will need to convert your installers in the same environment where you expect to deploy them. That way, the MSIX Packaging Tool will build the appropriate MSIX package for that environment. 



