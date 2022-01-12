---
description: An overview of topics for creating an MSIX package from source code
title: Building an MSIX package from your code 
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.custom: RS5
---

# Extend your packaged applications

MSIX makes it easy to extend your application using app extensions and optional packages. App extensions provide functionality similar to what plug-ins, add-ins, and add-ons do on other platforms. You can make your application an extension host to allowing it to consume content and deployment events from an extension packaged. App extensions were introduced in the Windows 10 Anniversary edition (version 1607, build 10.0.14393).

Optional packages are useful for dividing a large or complex app, or adding new components to an app that's already been published. With Visual Studio 2017, version 15.7 and .NET Native 2.1, you can load executable code from both C++ and C# optional packages.

App extensions are an open ecosystem and are intended for anyone to enhance your app. There is no gating or control over who gets to make an app extension. Optional packages are a closed ecosystem where you as the publisher decides who is allowed to make an optional package for your main package.

App extensions are also independent packages. They can be standalone apps and cannot have a deployment dependency on another app.  Optional packages require the primary package and cannot run without it.

|Topic| Description |
|:---|:---|
|[Creating and hosting an app extension](/windows/uwp/launch-resume/how-to-create-an-extension?context=%252fwindows%252fmsix%252frender)|This section discusses how to create and host an app extension in your MSIX package. |
[Custom properties for app extensions](custom-props-app-extensions.md)|This section discusses how to use custom properties for app extensions. |
|[Extending your app using optional packages](../package/optional-packages-with-executable-code.md)| This section discusses how to take advantage of the optional package model to load content into your main package. |
