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

## Best practices for environment setup
 
Make sure you have the [latest version of the MSIX Packaging Tool](mpt-overview.md#latest-public-version---1201910180). For the conversion process, there are a few other things that we recommend you consider before you start.

- The minimum OS version requirement for the MSIX Packaging Tool is Windows 10 1809. We understand that not everyone is on the Windows 10 October 2018 Update or even Windows 10. Therefore, we recommend that you create a clean VM that is pre-configured for the min version of support for MSIX.

- A clean machine for conversion is important because during the installation step of conversion using the MSIX Packaging Tool, we will be listening to everything on the machine to capture what the installer is doing, and it will help to prevent extraneous data in your package. A clean machine means that there aren't extraneous apps or services running on your machine. We recommend configuring the conversion machine to mimic the environment where the MSIX package will be run, so if there are services or policies that will be there, you can test that the package will actually work.

- If this isn't something you have we offer a **Quick Create VM**, the [MSIX Packaging Tool Environment](quick-create-vm.md) in Hyper-V, which is ready to go with conversion with Windows 10 1809 and the latest version of the MSIX Packaging Tool. 

- Follow the best practices recommendations for setting up the MSIX Packaging Tool, then create a checkpoint for the VM. This way you can use the VM to convert, revert to your previous checkpoint, and it will be a clean, configured machine ready for conversion again or for verifying your MSIX package converted successfully.

- Its also good to know what kind of dependencies you have so that you can understand which ones you should run with your app and which should be packaged as a modification package. For example, if you have runtime dependencies, it’s a good idea to include those in your main application. If you have a plug in, you should package that as a modification package to associate with your main application. 

## Best practices for setting up the MSIX Packaging Tool

Before you start any conversions we recommend configuring your settings in the MSIX Packaging Tool to simplify your workflow each time. 

### Tool Defaults
- **Generate a command line with each package** This setting will make it so you automatically generate a command line file so that if you are repackaging the same application (such as a new version) through the command line later, you can have a pre-configured command line file for that application. 
- **Select all fixes by default for prepare computer** This setting allows you to have all of the recommended fixes pre-selected so that during the prepare computer stage, you can simply choose to disable all without having to select them individually.
- **Enforce Microsoft Store versioning requirements** If you are planning to deploy your application through the Microsoft Store, you should ensure this is selected so that it conforms to the store requirements (this will affect the package version requirements and minimum OS version support). If this option is unchecked, the package will have a minimum version set to Windows 10 1709, and you will have full control over the 4 digits of the package version. If this option is checked, the package will have a minimum version set to Windows 10 1809 and the version must end in .0 (e.g. 1.5.6.0).
- **Add Package Integrity when generating a package** If this option is selected, Package Integrity will be automatically added to all packages generated. However, it will only apply to packages when Package Integrity is supported on the OS they are deployed on. 
- **Default save location** Specify the default save location where the generated packages and associated files will be saved.
- **Default installer browse location** Specify the default location to find installers to convert.
- **Server port number** Specify the server port number for the MSIX Packaging Tool. This is relevant if you are planning to convert using a [remote machine](https://docs.microsoft.com/windows/msix/packaging-tool/remote-conversion-setup). 
- **Environment preference** Specify the default environment for each conversion.
- **Signing preference** Specify the default option for signing when you are converting applications. It is required to sign your MSIX package in order to install it. You can choose from a few options for your signing preference.
    - Sign with Device Guard signing - we recommend this option if you don't have a trusted certificate in your enterprise. You can learn more about setting up Device Guard signing here. 
    - Sign with a certificate (.pfx) - We recommend this option if you already have a trusted certificate that you are using in your enterprise.
    - Specify a .cer file (does not sign) - If you do not wish to sign at the time of conversion, but want to ensure that the publisher information will be valid at the time of signing you can choose this option.
    - Do not sign package. - If you wish to sign your package using another method or at a later time after the package has been generated you can choose this option.

We also recommend that you add a timestamp server url to your siging preference (when applicable), so that your application can be installed, even if your certificate expires.
 
 ### Other Settings
 - **File and registry exlusions** While we have a default set of exclusion items, we recommend taking a look and adding or removing any exclusion items for your specific needs. 
 - **Installer exit codes** If you have specific installer exit codes that you want to trigger a restart during conversion, you can specify those here. By default we have common ones already added, but you can remove those if you never want restarts to be triggered. To note, a restart will never be triggered automatically by the Packaging Tool if you are using the UI, but it will if you are using the command line option. 
 
 You can also import or export your settings for sharing or ensuring conformity among peers, using these [instructions](https://docs.microsoft.com/windows/msix/packaging-tool/duplicate-mpt-settings-across-devices). 

## Best practices during repackaging

When you are using the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice during the repackaging process:

- When packaging ClickOnce installers, it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.
- When creating modification packages, you need to declare the package Name (identity name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.
- Performing the preparation steps in the **Prepare computer** page is optional but highly recommended, as this will help reduce any extraneous data in your package. 
- It is required that you sign your package in order to install it, but we also recommend that you timestamp your certificate so that your application can be installed, even if your certificate expires. 
- Declaring an installation location field in the **Package information** page is optional. Make sure that this path matches the installation location of application installer.

## Best practices while bundling MSIX packages

We recommend that you bundle MSIX packages when there are different installers available for different processor architectures such as x86, x64, and ARM for your application. By bundling the installers together, you can allow the Windows 10 OS to determine the applicability of the appropriate installer instead of the user having to make the correct selection. 

While [bundling MSIX packages](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages), you will need to have already converted your Win32 installers into MSIX packages. 

- The MSIX Packaging Tool will assume the processor architecture of the Windows 10 OS version that the conversion is taking place. For example, if you are on a x64 version of Windows 10, and you are converting a x86 version of an installers, the resulting MSIX package will be x64. 
- If you plan to bundle MSIX packages, you will need to convert your installers in the same environment where you expect to deploy them. That way, the MSIX Packaging Tool will build the appropriate MSIX package for that environment. 

## Best practices for testing your MSIX package

We recommend that you also test your MSIX package after conversion on a clean environment, as we specified above during environment setup. You should test your MSIX package on a different machine that has not installed the previous installer on it, so that you can ensure when you deploy your MSIX package, it has all of the components it needs and it isn't picking anything up from the previous installer. This can be achieved through a new VM, such as the Quick Create VM, or by reverting your conversion machine if you followed our checkpoint recommendation from above.

