---
Description: This guide provides a list of third-party products and installers to package desktop applications.
title: Package a desktop app using third-party installers
author: dianmsft
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Building an MSIX from your source code 
If your desktop app is in active development we recommend building an MSIX package in your build environment instead of generating an installer and running it through the MSIX packaging tool. Visual Studio 2017 version 15.4 and later (including Visual Studio 2019) you can use the App Packaging Project to package your desktop applications as an MSIX. For those not developing in Visual Studio we also have command line tools you can integrate into your build system to package your app binaries as an MSIX package.
If you're developing a UWP application, Visual Studio will default to MSIX as the packaging format for your application.

# Sign a Windows 10 app package

App package signing is a required step in the process of creating a Windows 10 app package that can be deployed. Windows 10 requires all applications to be signed with a valid code signing certificate.

To successfully install a Windows 10 application, the package doesn't just have to be signed but also trusted on the device. This means that the certificate has to chain to one of the trusted roots on the device. By default, Windows 10 trusts certificates from most of the certificate authorities that provide code signing certificates.

|Topic| Description |
|:---|:---|
|[Prerequisites for signing](sign-app-package-using-signtool.md#prerequisites)| This section discusses the prerequisites required to sign the Windows 10 app package. | 
|[Using SignTool](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to use SignTool from the Windows 10 SDK to sign the app package.|
|[Sign an MSIX package with Device Guard signing](https://docs.microsoft.com/windows/msix/package/signing-package-device-guard-signing)| This section discusses how to sign your app with Device Guard signing.|


* **Know what your application** The first thing to do is understand what your application is going to do. This is important to ensure that functionalities still work when you package your app as an MSIX. 

* **Package format**  A consideration to think about is whether you intend to have different app architectures in the same package. .msixbundles allow you to bundle multiple architecture versions of your installer in the same page. There are some caveats to this approach. Visit [MSIX Bundles](.../packaging-tool/bunle-msix-packages.md) to see your options. 

* **Extend your application** Applications that are packaged as an MSIX package have the ability to extend their apps. Depending on the scenario, application that are still in active developement can create app extensions and optional pacakges 

* **Allowing Enterprises to customize apps** Modification packages allow IT Pros to customize their application without having to repackage apps. That means that they can take a package directly from a developer without opening the package and go through the process of packaging their application. This is something to keep in mind when developing and packing your application. In the modification package it declare the your packaged app as its main app such that when your app launches it can view the virtual registry and virtual file system of the modification package such that the customization lights up. 

