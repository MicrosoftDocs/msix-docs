---
title: MSIX SDK 
description: MSIX SDK is an open source project that allows developers to use MSIX package format universally on all platforms.
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# MSIX SDK 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging" data-linktype="external">MSIX SDK on GitHub</a></p></div>

The MSIX SDK is an open source project that allows developers to use the MSIX package format universally on all platforms. This allows developers to build consistent experiences for their users on all platforms and distribute the experiences using the same package. The SDK provides guidance for developers to package their app content and build an app package manifest in a way that it can target the platforms of their choice. This enables developers to package their app content once instead of having to package for each platform.

The SDK provides the APIs required to verify, validate and unpack the package contents from the MSIX package. Using the project, app developers don't have to worry about whether the package has been tampered with or if it can be trusted. It will perform tamper protection and signature validation checks before the app contents are unpacked.

The SDK can be used by any cross platform client app that allows for third parties to build plugins or extensions. The client app developers can use the app extension model that is available on Windows 10 platform and use the MSIX SDK on the non-Windows 10 platforms. With the help of the SDK, third party developers building app extensions and plugins for the client app do not have to build a specific package for each platform. Instead, they build one package that is supported on Windows 10 and all the other platforms. With the SDK, app developers can choose specific platforms to support.

One of the key differentiators of the MSIX package is the manifest file. The manifest file contains all the metadata regarding the package and specifies all the key information that the client app can access to make appropriate choices like applicability or supportability. The manifest file allows client app developers and third party developers more options and flexibility to communicate characteristics such as requirements, availability, and support. For more information about how to use the manifest file to distribute an MSIX package to Windows 10 and non-Windows 10 platforms, see [this article](sdk-guidance.md).

## Get more info

MSIX SDK is an [open source project on GitHub](https://github.com/Microsoft/msix-packaging). The GitHub repo includes the full source and instructions for how to build the binaries for each platform.

If you have feedback, please submit it on the [MSIX tech community site](https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX). If there are issues or bugs that identified in the SDK, you can post them to the [issues page](https://github.com/Microsoft/msix-packaging/issues) for the MSIX SDK GitHub repo.