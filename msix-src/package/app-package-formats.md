---
title: App package formats
description: This article describes the different types of MSIX package formats that are useful for certain scenarios.
ms.date: 07/02/2019
ms.topic: article
keywords: windows 10, msix, uwp, desktop, package
ms.localizationpriority: medium
---

# App package formats

In addition to standard MSIX packages that contain a Windows app, there are several different types of specialized MSIX package formats that are useful for certain scenarios.

## Optional packages

Optional packages are used to either supplement or extend the original functionality of an app package. It's possible to publish an app, followed by publishing optional packages at a later time, or to publish both the app and optional packages simultaneously. By extending your app via an optional package, you have the advantages of distributing and monetizing content as a separate app package. Optional packages are typically intended to be developed by the original app developer, since they run with the identity of the main app (unlike app extensions). Depending on how you define your optional package, you can load code, assets, or code and assets from your optional package to your main app. If you need to enhance your app with content that can be monetized, licensed, and distributed separately, then optional packages might be the right choice for you. 

For more details, see [Optional packages and related set authoring](optional-packages.md).

## App streaming install

Streaming install is a way to optimize how your app is delivered to users. Rather than waiting for the entire app to download before you can use it, users can engage with the app as soon as a required portion has been downloaded. It's up to you, as a developer, to segment your app into a required section for basic activation and launch and additional content for the rest of the app. 

For more details, see [App streaming install](streaming-install.md).

## Flat bundle packages

Flat bundle app packages are similar to [regular app bundles](packaging-uwp-apps.md#types-of-app-packages), except that instead of including all of the app packages within the folder, the flat bundle only contains *references* to those app packages. By containing references to app packages instead of the files themselves, a flat bundle will reduce the amount of time it takes to package and download an app.

For more details, see [Flat bundle app packages](flat-bundles.md).

## Asset packages

Asset packages are a common, centralized source of executable, or non-executable files for use by your app. These are typically non-processor or language-specific files. For example, this might include a collection of pictures in one asset package, and videos in another asset package, both of which are used by the app. If your app supports multiple architectures and multiple languages, these assets could be included in the architecture package or resource package, but that also means the assets would be duplicated multiple times across the various architecture packages, taking up disk space. If asset packages are used, they only need to be included in the overall app package once. 

For more details, see [Introduction to asset packages](asset-packages.md).

## Resource packages

Resource packages are asset-only packages that allow your app to adapt to multiple display sizes and system languages. The resource package targets user language, system scale, and DirectX features, allowing the app to be tailored to a variety of user scenarios. Although an app package can contain several resources, the OS will only download the relevant resources per user device, saving bandwidth and disk space.

## App extensions

[App extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) enable your app to host content provided by other apps. Discover, enumerate, and access read-only content from those apps.

If an app supports extensions, any developer can submit an extension for the app. Thus, the host app needs to be robust when it loads an extension that it hasn't been pre-tested with. Extensions should be considered untrusted.

Applications cannot load code from extensions. If you need code execution, consider app services.

## App Services

Windows app services enable app-to-app communication by allowing your app to provide services to another app. App services let you create UI-less services that apps can call on the same device, and starting with Windows 10, version 1607, on remote devices. See [Create and consume an app service](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) for details.

App services are analogous to web services on a device. An app service runs as a background task in the host app and can provide its service to other apps. For example, an app service might provide a bar code scanner service that other apps could use. Or perhaps an Enterprise suite of apps has a common spell checking app service that is available to the other apps in the suite.

## Modification packages 
Modification packages allow IT Pros to customize apps without having to repackage. In Windows 10 version 1809 we introduced a new type of MSIX package called a *modification package*. Modification packages can also be plugins/add-ons that may not have an activation point. IT professionals can use this feature to flexibly change MSIX containers so that applications are overlaid by their enterprise's customizations. 

## See Also

[Create and consume an app service](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introduction to asset packages](asset-packages.md)  
[Package creation with the packaging layout](packaging-layout.md)  
[Optional packages and related set authoring](optional-packages.md)  
[Developing with asset packages and package folding](package-folding.md)  
[App streaming install](streaming-install.md)  
[Flat bundle app packages](flat-bundles.md)  
[Windows.ApplicationModel.AppService namespace](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
