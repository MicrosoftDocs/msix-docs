---
title: Version an MSIX package
description: Learn how MSIX package version numbers control updates and how to version preview and production packages.
ms.date: 08/26/2026
ms.topic: concept-article
keywords: msix, appx, package version, versioning, prerelease, update
---

# Version an MSIX package

Every MSIX package has a version in the `Version` attribute of the [`Identity`](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in its package manifest:

```xml
<Identity
    Name="Contoso.DesktopApp"
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"
    Version="1.4.12.0"
    ProcessorArchitecture="x64" />
```

The version belongs to the MSIX package, not to an individual application in the package. A package can contain multiple applications, but Windows deploys and updates the package as one unit.

## Version format

An MSIX package version uses four period-separated unsigned integers:

```text
Major.Minor.Build.Revision
```

Each part ranges from 0 through 65535. Use plain decimal integers without labels or leading zeros. For example, `0.1.0.0` and `1.12.3.4` are valid for non-Store packages, but `1.0.0-preview.1` and `0.1.01.0` aren't valid package versions.

Windows doesn't assign a semantic meaning to the four parts. Your organization can use the major, minor, build, and revision positions to match its release process. What matters to deployment is the numeric ordering of the complete four-part value. Comparison starts with the major part, then proceeds from left to right. For example:

- `2.0.0.0` is higher than `1.65535.65535.65535`.
- `1.10.0.0` is higher than `1.9.999.999`.
- `1.0.1.0` is higher than `1.0.0.500`.

## Package versions and updates

For a normal update, keep the package `Name` and `Publisher` unchanged and assign a version higher than the installed version. Windows treats packages with a different name or publisher as a different package family, not as an update.

Windows can update directly between nonconsecutive versions. You don't have to publish or install every intermediate version. For information about how Windows minimizes the update download, see [App package updates](../app-package-updates.md).

Installing a lower version isn't allowed by default. Some deployment tools support an explicit force-update option for rollback scenarios, but don't rely on that option as your normal versioning strategy.

## Version preview releases

MSIX versions contain numbers only, so they don't support Semantic Versioning labels such as `-alpha`, `-beta`, or `-rc`.

Choose one of these strategies for preview releases:

- **Use the production package identity.** Assign preview builds numeric versions lower than the planned production version. The production package must have a higher version than every preview that it replaces. For example, use `1.0.0.1` through `1.0.0.20` for previews and `1.0.1.0` for the production release.
- **Use a separate preview package identity.** Give the preview package a different `Name`, such as `Contoso.DesktopApp.Preview`. Windows then treats preview and production as separate package families, which allows side-by-side installation. Because they are separate, the production package doesn't update or remove the preview package, and package data isn't shared automatically.

A separate identity is usually the clearer choice when users need preview and production applications side by side.

## Microsoft Store version requirements

The Microsoft Store applies additional version rules. For Windows 10 and Windows 11 packages, the major part must be greater than zero, and the revision part must be `0` when you submit the package. The Store can change the revision value while processing the submission.

If you distribute outside the Store, all four parts are available for your versioning scheme. If you distribute through the Store, design your scheme around the Store requirements from the start. For the current submission rules, see [App package requirements for MSIX apps](/windows/apps/publish/publish-your-app/msix/app-package-requirements#package-version-numbering).
