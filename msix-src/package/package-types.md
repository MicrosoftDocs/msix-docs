---
title: MSIX package types
description: Understand Main, Framework, Resource, Optional, and Bundle packages, their manifest indicators, and how to identify each type.
ms.date: 09/30/2026
ms.topic: concept-article
keywords: msix, package type, main package, framework package, resource package, optional package, bundle
---

# MSIX package types

MSIX supports four package types: Main, Framework, Resource, and Optional. MSIX also supports Bundle packages, which are distribution containers for one or more MSIX packages.

A package type defines a combination of deployment, dependency, execution, and servicing behavior. Some behavior is inherent to the type, while a manifest declaration can override other defaults.

| Type | Primary purpose | Manifest or identity indicator |
|---|---|---|
| **Main** | Primary package for an application | No declaration for another package type |
| **Framework** | Shared libraries, components, or runtime content | `<Framework>true</Framework>` |
| **Resource** | Language-, scale-, or qualifier-specific resources | `<ResourcePackage>true</ResourcePackage>` |
| **Optional** | Additional content associated with one Main package | `<uap3:MainPackageDependency>` |
| **Bundle** | Distribution container for multiple MSIX packages | Bundle manifest and a `ResourceId` value of `~` |

## Main packages

A Main package represents the primary package for an application. It can declare applications, capabilities, dependencies, extensions, and out-of-process servers.

The manifest doesn't contain an explicit Main-package marker. A package is a Main package when it doesn't declare itself as another package type.

## Framework packages

A Framework package provides shared libraries, components, resources, or other runtime content. Other packages or processes add the Framework package to their package graph before loading its content.

Framework packages support side-by-side versions so applications can move to newer versions independently. A package declares this type by setting the [Framework](/uwp/schemas/appxpackage/uapmanifestschema/element-f-framework) manifest element to `true`.

For information about using Framework packages from packaged and unpackaged processes, see [Use the dynamic dependency API](/windows/apps/desktop/modernize/framework-packages/use-the-dynamic-dependency-api).

## Resource packages

A Resource package separates language-, display-scale-, or other qualifier-specific resources from a Main or Optional package. Windows installs only the resource packages that apply to a device or user.

Resource packages don't permit execution by default. A package can override this default with [uap6:AllowExecution](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution). A package declares the Resource type by setting [ResourcePackage](/uwp/schemas/appxpackage/uapmanifestschema/element-f-resourcepackage) to `true`.

## Optional packages

An Optional package contains additional content for a specific Main package. For example, an application can put downloadable maps, high-resolution assets, or optional tools in separate packages.

Unlike a Framework package, an Optional package is associated with one Main package family. It declares that relationship through [uap3:MainPackageDependency](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-mainpackagedependency).

For authoring and related-set requirements, see [Optional packages and related set authoring](optional-packages.md).

## Bundle packages

An MSIX bundle (`.msixbundle`) contains one or more MSIX packages. A bundle provides one distribution artifact while allowing Windows to install only the architecture and resource packages that apply to the target device.

All packages in a bundle belong to the same package family. A bundle is a container rather than a fifth deployable package type.

## Identify package types in code

The [Windows.ApplicationModel.Package](/uwp/api/windows.applicationmodel.package) class exposes properties that identify a package type:

```csharp
static string GetPackageType(Windows.ApplicationModel.Package package)
{
    if (package.IsBundle)
    {
        return "Bundle";
    }

    if (package.IsFramework)
    {
        return "Framework";
    }

    if (package.IsResourcePackage)
    {
        return "Resource";
    }

    if (package.IsOptional)
    {
        return "Optional";
    }

    return "Main";
}
```

## Identify package types with PowerShell

By default, `Get-AppxPackage` returns Main and Framework packages. Use `-PackageTypeFilter` to include other types:

```powershell
Get-AppxPackage -PackageTypeFilter Main,Framework,Resource,Optional,Bundle |
    Select-Object Name, PackageFullName, IsFramework, IsResourcePackage, IsBundle
```

The output object doesn't expose an `IsOptional` property. Use the filter to include Optional packages, and inspect their manifest or package relationship when you need to distinguish them.

## Related content

- [App package formats](app-package-formats.md)
- [Framework packages](fx-packages.md)
- [Resource packages](resource-package.md)
- [Optional packages](optional-packages.md)
- [PackageType: Main, Framework, Resource, Optional, Bundle](https://devblogs.microsoft.com/insidemsix/msix-package-types/)
