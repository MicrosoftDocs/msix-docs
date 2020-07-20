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

If you haven't already configured your environment for conversion, you can follow our [environment best practices](prepare-your-environment.md) recommendations and then come back here to set up the MSIX Packaging Tool. Before you start any conversions we recommend configuring your settings in the MSIX Packaging Tool to simplify your workflow each time. Launch the MSIX Packaging Tool and then go to the settings (gear in the top right of the landing page) to configure your tool defaults. 

### Configure your MSIX Packaging Tool defaults

- **Generate a command line with each package** This setting will make it so you automatically generate a command line template file so that if you are repackaging the same application (such as a new version) through the command line later, you can have a pre-configured command line template file for that application. You will need to provide an installer in order to generate a template file during the workflow.
- **Select all fixes by default for prepare computer** This setting allows you to have all of the recommended fixes pre-selected so that during the prepare computer stage, you can simply choose to disable all without having to select them individually.
- **Enforce Microsoft Store versioning requirements** If you are planning to deploy your application through the Microsoft Store, you should ensure this is selected so that it conforms to the store requirements (this will affect the package version requirements and minimum OS version support). If this option is unchecked, the package will have a minimum version set to Windows 10 1709, and you will have full control over the 4 digits of the package version. If this option is checked, the package will have a minimum version set to Windows 10 1809 and the version must end in .0 (e.g. 1.5.6.0).
- **Add Package Integrity when generating a package** If this option is selected, Package Integrity will be automatically added to all packages generated. [Package Integrity](../package/signing-package-overview.md#package-integrity-enforcement) is supported on Windows 10 2004 and later.
- **Add support for MSIX Core when generating a package** This option allows you to add [MSIX Core](../msix-core/msixcore.md) support to every package that you generate. Once selected, this will offer a dropdown that will allow you to specify the Windows version to support. 
- **Default save location** Specify the default save location where the generated packages and associated files will be saved.
- **Default installer browse location** Specify the default location to find installers to convert.
- **Server port number** Specify the server port number for the MSIX Packaging Tool. This is relevant if you are planning to convert using a [remote machine](remote-conversion-setup.md). 
- **Environment preference** Specify the default environment for each conversion.
- **Signing preference** Specify the default option for signing when you are converting applications. It is required to sign your MSIX package in order to install it. You can choose from a few options for your signing preference.
    - Sign with Device Guard signing - we recommend this option if you don't have a trusted certificate in your enterprise. There are some steps to enable [Device Guard signing](../package/signing-package-device-guard-signing.md) that you need to take before choosing this option. 
    - Sign with a certificate (.pfx) - We recommend this option if you already have a trusted certificate that you are using in your enterprise.
    - Specify a .cer file (does not sign) - If you do not wish to sign at the time of conversion, but want to ensure that the publisher information will be valid at the time of signing you can choose this option.
    - Do not sign package. - If you wish to sign your package using another method or at a later time after the package has been generated you can choose this option.
    We also recommend that you add a **timestamp server url** to your signing preference (when applicable), so that your application can be installed, even if your certificate expires.   

> [!NOTE]
> Signing an MSIX package format application with a SHA1 certificate is not supported.

### Other Settings

- **File and registry exclusions** While we have a default set of exclusion items, we recommend taking a look and adding or removing any exclusion items for your specific needs. 
- **Installer exit codes** If you have specific installer exit codes that you want to trigger a restart during conversion, you can specify those here. By default we have common ones already added, but you can remove those if you never want restarts to be triggered. To note, a restart will never be triggered automatically by the Packaging Tool if you are using the UI, but it will if you are using the command line option. 
 
You can also import or export your settings for sharing by using these [instructions](duplicate-tool-settings-across-devices.md). 

## Best practices during repackaging

When you are using the MSIX Packaging Tool, there are a few things that we also recommend you do as best practice during the repackaging process:

- When packaging ClickOnce installers, it is necessary to send a shortcut to desktop if the installer is not doing so already. In general, it is good practice to always remember to send a shortcut to desktop for the main app executable.
- When creating modification packages, you need to declare the package Name (identity name) of the parent application in the tool UI so that the tool sets the correct package dependency in the manifest of the modification package.
- Performing the preparation steps in the **Prepare computer** page is optional but highly recommended, as this will help reduce any extraneous data in your package.
- It is required that you sign your package in order to install it, but we also recommend that you timestamp your certificate so that your application can be installed, even if your certificate expires.
- Declaring an installation location field in the **Package information** page is optional. Make sure that this path matches the installation location of application installer.

## Best practices for testing your MSIX package

We recommend that you also test your MSIX package after conversion on a clean environment, as we specified during environment setup. You should test your MSIX package on a different machine that has not installed the previous installer on it, so that you can ensure when you deploy your MSIX package, it has all of the components it needs and it isn't picking anything up from the previous installer. This can be achieved through a new virtual machine, such as the [Quick Create VM](Quick-Create-VM.md), or by reverting your conversion machine if you created a checkpoint before starting conversion.
