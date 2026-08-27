---
title: MSIX package identity
description: Learn how MSIX package identity, package full names, package family names, publisher IDs, and application identity are constructed and used.
ms.date: 08/27/2026
ms.topic: concept-article
keywords: msix, package identity, package full name, package family name, publisher id, aumid
---

# MSIX package identity

Windows uses package identity to uniquely identify and manage an MSIX package. The [`Identity`](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the package manifest declares five fields:

- **Name**: The package name.
- **Version**: A four-part package version.
- **ProcessorArchitecture**: The architecture the package targets, such as `x64`, `arm64`, or `neutral`.
- **ResourceId**: An identifier used to distinguish resource packages. This value is usually empty for a main package.
- **Publisher**: The package publisher distinguished name. For a normally signed package, it must match the subject of the certificate used to sign the package. An unsigned test package instead uses the required unsigned-publisher marker; see [Create an unsigned MSIX package](unsigned-package.md).

Together, these fields form the package identity. Changing the package contents requires a new identity, which usually means incrementing the version.

## Package full name

The package full name is an opaque string derived from all five identity fields:

`<Name>_<Version>_<Architecture>_<ResourceId>_<PublisherId>`

For example:

`Contoso.Reader_2.1.0.0_x64__8wekyb3d8bbwe`

In this example, the resource ID is empty, which produces two adjacent underscores before the publisher ID. The package full name identifies one specific package version and architecture. Windows uses it in deployment APIs and package installation paths.

Don't construct or parse a package full name manually. Use the [`PackageId.FullName`](/uwp/api/windows.applicationmodel.packageid.fullname) property or the [package identity APIs](/windows/win32/appxpkg/package-identity-overview) instead.

## Package family name

The package family name is derived from the package name and publisher ID:

`<Name>_<PublisherId>`

For example:

`Contoso.Reader_8wekyb3d8bbwe`

A package family name doesn't contain the version, architecture, or resource ID. It therefore identifies the related set of packages across versions and architectures. Windows scopes package data and many security assignments to the package family.

Package updates occur within the same package family. To update an installed MSIX package, the new package must keep the same name and publisher identity. By default, Windows updates a registered package only with a higher package version in the same package family. Deployment operations that explicitly request [`ForceUpdateFromAnyVersion`](/powershell/module/appx/add-appxpackage#-forceupdatefromanyversion) can install a lower version; use that option only when the application data and schema are compatible with the downgrade. Architecture and resource package selection can change between applicable versions without changing the package family name.

Changing the name or publisher creates a different package family and Windows treats the package as a separate product. For certificate migration options that preserve the package family, see [MSIX persistent identity](persistent-identity.md).

## Publisher ID

The publisher ID is a 13-character value that Windows derives from the `Publisher` field. It provides a compact, path-safe representation of the certificate subject name. It isn't the publisher string itself and shouldn't be chosen or edited independently.

Use [`PackageId.PublisherId`](/uwp/api/windows.applicationmodel.packageid.publisherid) to retrieve it. Native applications can use functions such as [`PackageFamilyNameFromId`](/windows/win32/api/appmodel/nf-appmodel-packagefamilynamefromid) to derive package identity strings.

## Package identity and application identity

A package is the unit that Windows deploys, updates, and removes. A package can contain multiple applications or no applications.

Each application declared in a package has an application identity represented by an Application User Model ID (AUMID). Windows uses the AUMID to identify a user-facing application for activation, notifications, taskbar grouping, and other runtime behavior. Package identity identifies the deployment artifact; the AUMID identifies an application within that package.

For a packaged application, the AUMID has the form `<PackageFamilyName>!<ApplicationId>`, where `ApplicationId` is the `Id` value of the corresponding [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) element. For example, an application with `Id="Reader"` in `Contoso.Reader_8wekyb3d8bbwe` has the AUMID `Contoso.Reader_8wekyb3d8bbwe!Reader`. Use [`Get-StartApps`](/powershell/module/startlayout/get-startapps) or the appropriate application model API to retrieve registered AUMIDs rather than deriving them from an installation path.

For a deeper technical explanation, see [Package Identity](https://devblogs.microsoft.com/insidemsix/package-identity/) and [Applications are not packages](https://devblogs.microsoft.com/insidemsix/applications-are-not-packages/) on the Inside MSIX blog.
