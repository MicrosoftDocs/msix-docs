# MSIX SDK Overview page 

## What is it?

MSIX SDK is an open source project that allows developers to use MSIX package format universally on all platforms. This allows the developers to build consistent experience for their users on all platforms and distribute the experience using the same package. The SDK provides guidance for developers to package their app content and build app package manifest in a way it can target the platforms for their choice. This enables developers to package their app content once instead of having package for each platform. 

The SDK provides the APIs required to verify, validate and unpack the package contents from the MSIX package. Using the SDK, app developers don't have to worry about whether the package has been tampered with or if it can be trusted. The SDK will perform tamper protection and signature validation checks before the app contents are unpacked. 

The SDK can be used by any cross platform client app that allows for third parties to build plugins or extensions. The client app developers can use the app extension model that is available on Windows 10 platform and use the MSIX SDK on the non-Windows 10 platforms. With the help of the SDK, third party developers building app extensions and plugins for the client app do not have to build a specific package for each platform - they build one package and that package is supported on Windows 10 and all the others platforms. With the SDK, app developers can choose specific platforms to support. 

One of the key differentiators of the MSIX package is the manifest file. The manifest file contains all the metadata regarding the package and specifies all the key information that the client app can access to make appropriate choices like applicability or supportability. The manifest file allows the client app developers and third party developers more options and flexibility to communicate the requirements, availability, support etc. 

## Get more info

MSIX SDK is an open source project on GitHub. Here you access to the full source and instructions on how to build the binaries for each platform are available here. 

If you have feedback please post on our tech community page. If there are issues or bugs that identified in the SDK, you can post them directly on the GitHub issues page. 



