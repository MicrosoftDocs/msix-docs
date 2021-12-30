---
title: prepare your environment for conversion
description: Provides an overview of how to convert an MSIX from an existing installer.
ms.date: 01/27/2020
ms.topic: article
keywords: msix
---

# Prepare your environment for conversion

Preparing your conversion environment is an important step in the conversion process. The following recommendations will help to ensure your success when you are converting your existing installers to MSIX.

- The minimum OS version requirement for the MSIX Packaging Tool is Windows 10 1809. We understand that not everyone is on the Windows 10 October 2018 Update or even Windows 10. Therefore, we recommend that you create a clean VM that is pre-configured for the min version of support for the MSIX Packaging Tool. Deployment of the generated MSIX package has different support requirements.

- A clean machine for conversion is important because during the installation step of the MSIX Packaging Tool, we will be listening to everything in the environment to capture what the installer is doing. A clean machine means that there aren't extraneous apps or services running on your machine that could get captured in your package.

- We recommend configuring the conversion machine to mimic the environment where the MSIX package will be run, so if there are services or policies that will be there, you can test that the package will actually work.

- Your conversion environment should match the architecture of where you will be deploying the application. For example, if you are going to deploy your MSIX package on an x64 machine, you should perform the conversion on an x64 machine. 

- If this isn't something you have we offer a **Quick Create VM**, the [MSIX Packaging Tool Environment](quick-create-vm.md) in Hyper-V, which is ready to go with conversion with Windows 10 1809 and the latest version of the MSIX Packaging Tool. 

- Follow the best practices recommendations for [setting up the MSIX Packaging Tool](tool-best-practices.md) (or tool of your choice), then create a checkpoint for the VM. This way you can use the VM to convert, revert to your previous checkpoint, and it will be a clean, configured machine ready for conversion again or for verifying your MSIX package converted successfully.

- Its also good to know what kind of dependencies you have so that you can understand which ones you should run with your app and which should be packaged as a modification package. For example, if you have runtime dependencies, it’s a good idea to include those in your main application. If you have a plug in, you should package that as a modification package to associate with your main application.

- If you want to perform your conversion on a remote machine, you will need to do some additional [set-up](remote-conversion-setup.md) to enable it for conversion.
