---
title: External dependencies for MSIX packages
description: Understand the supported scope and limitations of the win32dependencies ExternalDependency manifest element.
ms.date: 08/27/2026
ms.topic: concept-article
keywords: msix, external dependency, webview2, app installer
---

# External dependencies for MSIX packages

The [`win32dependencies:ExternalDependency`](/uwp/schemas/appxpackage/uapmanifestschema/element-win32dependencies-externaldependency) manifest element can request installation of an allowlisted dependency that isn't included in an MSIX package. The App Installer app retrieves and installs the dependency when the required minimum version isn't already present.

Use this element only when your distribution flow and dependency are supported. It isn't a general-purpose dependency bootstrapper.

## Supported scope

`win32dependencies:ExternalDependency` is processed only when the MSIX package is installed by the App Installer app. This includes supported installations in which the App Installer app opens an MSIX package or processes an [App Installer file](../app-installer/app-installer-root.md), including supported `ms-appinstaller` protocol activations.

The element is ignored when the package is deployed by another mechanism, including:

- A `PackageManager` API.
- An Appx or DISM PowerShell cmdlet.
- Microsoft Intune or another enterprise management system.
- A Microsoft Store deployment flow that doesn't use the App Installer app.

The external component isn't represented as an MSIX package dependency in the package graph. Don't rely on Windows package dependency resolution to validate or service it after deployment. The application should verify the required component and version before using it.

## Validation and allowlist limitations

MSIX packaging tools don't validate this element at build time. `makeappx.exe`, PackageWriter, and AppxManifestReader can create or read a package without confirming that the dependency name, publisher, or version can be resolved by the App Installer app.

Only dependencies on the Microsoft-maintained allowlist are supported. Currently, the element supports the Microsoft Edge WebView2 Runtime. Use the exact `Name` and `Publisher` values documented in the [manifest schema reference](/uwp/schemas/appxpackage/uapmanifestschema/element-win32dependencies-externaldependency#allowed-external-dependencies).

The target device must have App Installer version 1.16.12651.0 or later. Dependency acquisition also requires network access unless the required version is already installed. If `Optional` is `true`, an offline installation can continue without the dependency; your application must still handle the missing dependency.

## Choose a dependency model

If the dependency is available as an MSIX [framework package](fx-packages.md) and the application is designed to load it through the package graph, declare a package dependency and deploy the framework through every supported channel. For an unpackaged runtime such as the WebView2 Runtime, use that runtime's [supported distribution method](/microsoft-edge/webview2/concepts/distribution) for channels that don't use the App Installer app.

Use `win32dependencies:ExternalDependency` only when all of the following conditions apply:

- You distribute through the App Installer app.
- The dependency is on the supported allowlist.
- Your application checks for the dependency at runtime.
- You have a fallback or recovery experience if acquisition doesn't occur.
