---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Package from the command line
description: This section contains or links to articles about manually packaging Windows apps.
ms.date: 01/30/2020
ms.topic: article
keywords: windows 10, uwp, packaging
---

# Package from the command line

If you don't develop your app in Visual Studio, you can use the MSIX command line tools to package and sign your applications.


## Purpose

This section links to articles about manually packaging your app as an MSIX using command line tools.

| Topic | Description |
|-------|-------------|
| [Generating package components ](../desktop/desktop-to-uwp-manual-conversion.md) | Create a package manifest and add Target-based unplated assets (optional) |
| [Create an MSIX package or bundle with the MakeAppx.exe tool](create-app-package-with-makeappx-tool.md) | MakeAppx.exe creates, encrypts, decrypts, and extracts files from app packages and bundles. |
| [Create a certificate for package signing](create-certificate-package-signing.md) | Create and export a certificate for app package signing with PowerShell tools. |
| [Sign an app package using SignTool](sign-app-package-using-signtool.md) | Use SignTool to manually sign an app package with a certificate. |

### Advanced topics

This section contains more advanced topics for componentizing a large and/or complex app for more efficient packaging and installation. 

> [!IMPORTANT]
> If you intend to submit your app to the Store, you need to contact [Windows developer support](https://developer.microsoft.com/windows/support) and get approval to use any of the advanced features in this section.


| Topic | Description |
|-------|-------------|
| [Package creation with the packaging layout](packaging-layout.md) | The packaging layout is a single document that describes packaging structure of the app. It specifies the bundles of an app (primary and optional), the packages in the bundles, and the files in the packages. |
| [Introduction to asset packages](asset-packages.md) | Asset packages are a type of package that act as a centralized location for an application’s common files – effectively eliminating the necessity for duplicated files throughout its architecture packages. |
| [Developing with asset packages and package folding](package-folding.md) | Learn how to efficiently organize your app with asset packages and package folding. |
| [Flat bundle app packages](flat-bundles.md) | Describes how to create a flat bundle for your app’s package files. |

