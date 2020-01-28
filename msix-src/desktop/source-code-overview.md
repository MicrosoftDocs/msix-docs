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

# Key concepts to know before you package your app 
Before packaging your application there are a few things to consider when packaging your application. 

* **Know what your application** The first thing to do is understand what your application is going to do. This is important to ensure that functionalities still work when you package your app as an MSIX. 

* **Package format**  A consideration to think about is whether you intend to have different app architectures in the same package. .msixbundles allow you to bundle multiple architecture versions of your installer in the same page. There are some caveats to this approach. Visit [MSIX Bundles](.../packaging-tool/bunle-msix-packages.md) to see your options. 

* **Extend your application** Applications that are packaged as an MSIX package have the ability to extend their apps. Depending on the scenario, application that are still in active developement can create app extensions and optional pacakges 

* **Allowing Enterprises to customize apps** Modification packages allow IT Pros to customize their application without having to repackage apps. That means that they can take a package directly from a developer without opening the package and go through the process of packaging their application. This is something to keep in mind when developing and packing your application. In the modification package it declare the your packaged app as its main app such that when your app launches it can view the virtual registry and virtual file system of the modification package such that the customization lights up. 

