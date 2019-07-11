---
title: Optional packages and related sets
description: This article lists resources for learning about installing related sets via App Installer.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Optional packages and related sets

Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC), dividing a large app for size restraints, or for shipping any additional content separate from your original app.

Related sets are an extension of optional packages. They allow you to enforce a strict set of versions across main and optional packages. They also let you load native code (C++) from optional packages.

For more information about these types of packages, see [Optional packages and related set authoring](https://docs.microsoft.com/windows/uwp/packaging/optional-packages). If you're just starting out with optional packages or related sets, the following additional blog posts are good resources to get started:

1.  [Extend your application using optional packages](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [Build your first optional package](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [Loading code from an optional package](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [Tooling to create a related set](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)

Starting with the Windows 10 Fall Creators Update (Windows 10, version 1709), related sets can now be installed via App Installer. This allows distribution and deployment of related set app packages to users. For more information about how to create an App Installer file to define a related set, see [Create an App Installer file manually](how-to-create-appinstaller-file.md).
