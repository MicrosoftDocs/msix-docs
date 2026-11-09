---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Optional packages and related set authoring
description: Optional packages contain content that can be integrated with a main package. These are useful for downloadable content (DLC) and other scenarios.
ms.date: 08/26/2026
ms.topic: article
author: andreww-msft
ms.author: jken
keywords: windows 10, msix, uwp, optional packages, related set, package extension, visual studio
---

# Optional packages and related set authoring

An optional package contains content or executable code that a main MSIX package can use. Optional packages are useful for downloadable content (DLC), feature modules, and content that you want to install or service separately from the main package. The optional packages run in the main package's MSIX container.

This article uses the [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample) to show the complete development flow. The sample has a main application, a content-only optional package, and two activatable optional packages in a related set.

## Choose an optional-package architecture

Decide whether the optional packages can be serviced independently before you create the projects.

| Architecture | Use it when | Version behavior |
| --- | --- | --- |
| Independent optional package | The main application can discover and use any compatible installed version of the optional package. Examples include optional content and DLC. | The main and optional packages can be installed and updated separately. Your code must tolerate the optional package being absent. |
| Related set | The main package and specific optional-package versions form one tested release and must become available together. | The main bundle identifies the optional packages and versions in the set. Windows doesn't make a new set available until all packages required by that version are installed. |

A related set is stricter than an independent optional package. Use it when mixing package versions could break the application, not only as a way to install several packages at once.

## Prerequisites

- Visual Studio with the Universal Windows Platform development workload
- The **C++ (v14x) Universal Windows Platform tools** component, to build the sample's C++ optional packages that carry executable code
- Windows 10, version 1703 or later
- Windows 10, version 1703 SDK or later

Set **Target Platform Min Version** to `10.0.15063.0` or later for every project. To get the current development tools, see [Downloads and tools for Windows](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Submitting an application that uses optional packages or related sets to the Microsoft Store requires permission. You can use these package types for line-of-business or enterprise distribution without Partner Center permission when you don't submit them to the Store. To request Store submission permission, contact [Windows developer support](https://developer.microsoft.com/windows/support).

## Create the projects and manifests

Create a packaging project for the main package and a packaging project for each optional package. An optional package has its own package identity, version, and payload, but its manifest declares the main package that it extends.

1. In the main package's `Package.appxmanifest`, find the `Name` attribute of the `Identity` element. Use the identity name, not the complete package family name that includes the publisher ID suffix.
1. Open the optional package's `Package.appxmanifest` as XML.
1. Make sure the manifest declares the `uap3` namespace and includes it in `IgnorableNamespaces`.
1. Add `uap3:MainPackageDependency` under `Dependencies`, and set `Name` to the main package's identity name:

    ```xml
    <Package
        xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
        IgnorableNamespaces="uap3">
      ...
      <Dependencies>
        <TargetDeviceFamily
            Name="Windows.Universal"
            MinVersion="10.0.15063.0"
            MaxVersionTested="10.0.15063.0" />
        <uap3:MainPackageDependency Name="Contoso.MainApp" />
      </Dependencies>
      ...
    </Package>
    ```

For a content-only package, omit user-facing entry points or set `AppListEntry="none"`. If the optional package provides an application that users can launch, declare its application entry point and visual elements. Compare the `OptionalPackage` and `ActivatableOptionalPackage1` manifests in the sample.

> [!NOTE]
> For non-Store distribution, an optional package can have a different publisher. Add `xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"`, include `uap4` in `IgnorableNamespaces`, and use `uap4:MainPackageDependency` in place of the uap3 element, for example: `<uap4:MainPackageDependency Name="Contoso.MainApp" Publisher="CN=Contoso" />`. Because the `Publisher` attribute is defined in the uap4 schema, this scenario requires Windows 10, version 1709 (10.0.16299.0) or later and the 1709 SDK. Different publishers aren't supported for Microsoft Store submissions.

## Configure a related set

For a related set, add a `Bundle.Mapping.txt` file to the main package project. The file lists the optional package projects whose built packages must be referenced by the main bundle:

```text
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"
```

Visual Studio uses this mapping to add the related-set metadata to the main bundle's `AppxBundleManifest.xml`. Keep paths relative to the main package project, and list only optional packages that must participate in the version-locked set.

Also configure project dependencies so that the main and optional projects build in the correct order:

1. In Visual Studio, right-click the solution, and select **Project Dependencies**.
1. Select each optional package project and make it depend on the main application project.
1. Build the solution and inspect the generated main bundle manifest to confirm that it references every optional package listed in `Bundle.Mapping.txt`.

## Build and deploy during development

For an independent optional package:

1. Build and deploy the main package.
1. Build and deploy the optional package.
1. Launch the main application and verify both the feature-present and feature-absent paths.

For the related-set projects in the sample, deploying `MyMainApp` also deploys `ActivatableOptionalPackage1` and `ActivatableOptionalPackage2`. Build and deploy `OptionalPackage` separately because it isn't listed in `Bundle.Mapping.txt`. For a production deployment, distribute the related set together by using an [App Installer file](../app-installer/how-to-create-appinstaller-file.md) that identifies the main and optional packages.

## Access optional content and code

The main application must treat an optional package as optional. Enumerate [`Package.Current.Dependencies`](/uwp/api/windows.applicationmodel.package.dependencies), select the entries where [`IsOptional`](/uwp/api/windows.applicationmodel.package.isoptional) is `true`, and read files from each package's [`InstalledLocation`](/uwp/api/windows.applicationmodel.package.installedlocation). The sample's **Load Content from Optional Packages** action demonstrates this pattern for content-only and activatable packages.

Don't assume that the optional package is installed, at a particular path, or at the same version as the main package unless it belongs to a related set. Keep a built-in fallback for missing optional content. To load a DLL or Windows Runtime component from an optional package, follow [Optional packages with executable code](optional-packages-with-executable-code.md); executable-code packages have additional build and loading requirements.

## Plan versioning and servicing

Each package keeps its own four-part version. Use these servicing rules:

- **Independent optional packages:** Update the main and optional packages independently. Keep the dependency contract backward compatible, and test the main application with the optional package absent, at the oldest supported version, and at the new version.
- **Related sets:** Treat the main bundle and all optional packages listed in `Bundle.Mapping.txt` as one release. Increment and build the applicable packages, generate a new main bundle that references the intended optional-package versions, and publish every package in the set to the same deployment location. Windows can stage packages independently, but it doesn't activate the new related set until all packages required by the main bundle are present.
- **Identity:** Don't change an existing package's identity name or publisher during an update. Those values establish the package relationship and signing trust.

Before publishing an update, inspect the generated bundle manifest and the App Installer file to make sure their names, publishers, versions, processor architectures, and URIs match the artifacts you deploy.

## Remove optional packages

Users can remove optional packages in Windows **Settings**. An application can also use [PackageCatalog.RemoveOptionalPackagesAsync](/uwp/api/windows.applicationmodel.packagecatalog.removeoptionalpackagesasync) to remove packages:

```csharp
PackageCatalog catalog = PackageCatalog.OpenForCurrentPackage();
var packageFamilyNames = new List<string>
{
    "FabrikamAgeAnalysis_kwpnjs8c36mz0"
};

var result = await catalog.RemoveOptionalPackagesAsync(packageFamilyNames);
if (result.ExtendedError != null)
{
    throw result.ExtendedError;
}
```

> [!NOTE]
> Removing a package from a related set can require the platform to restart the main application so that it doesn't continue to use content from the removed package. Notify the user before calling the API. For a content-only independent optional package, release references to its content before removal.

## Limitations

- Optional packages require Windows 10, version 1703 or later.
- Store submission requires permission, and Store packages can't use different publishers for the main and optional packages.
- The main application must handle an independent optional package being unavailable or removed.
- Visual Studio doesn't support directly debugging a related-set optional project. Deploy without debugging, start the main application, and use **Debug** > **Attach to Process** to attach to the main application process.
- Optional packages aren't a general plug-in security boundary. Their code and content run in the main package's MSIX container.

## Next steps

- Work through the [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample).
- Learn how to [load executable code from an optional package](optional-packages-with-executable-code.md).
- Create an [App Installer file for a related set](../app-installer/how-to-create-appinstaller-file.md).
- Review the generated [bundle manifest schema](/uwp/schemas/bundlemanifestschema/bundle-manifest).
- Read the Inside MSIX articles about [building an optional package](/archive/blogs/appinstaller/build-your-first-optional-package) and [creating a related set](/archive/blogs/appinstaller/tooling-to-create-a-related-set).
