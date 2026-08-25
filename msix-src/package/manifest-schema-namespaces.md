---
title: Package manifest schema namespaces
description: Learn how the MSIX package manifest schema uses XML namespaces.
ms.date: 08/25/2026
ms.topic: article
keywords: windows 11, windows 10, msix, package manifest, schema, namespaces
---

# Package manifest schema namespaces

The package manifest is an XML document that tells Windows how to deploy, display,
and update an MSIX package. The package manifest schema is versioned through XML
namespaces. The default foundation namespace describes the root package structure,
and prefixed namespaces such as `uap`, `desktop`, `com`, and `rescap` add schema
features for specific Windows app model areas.

This article explains how to choose, declare, and use package manifest namespaces.
It doesn't replace the element-by-element schema reference. For element syntax,
attributes, child elements, and element-specific requirements, use the canonical
[Package manifest schema reference for Windows 10](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).
Individual elements are documented under that reference tree.

## Why package manifest namespaces are versioned

Higher-numbered namespaces exist because Windows added package manifest features in
successive Windows releases. For example, later `uap`, `desktop`, and `com`
namespaces add newer extension points or attributes while preserving earlier schema
contracts.

A higher number doesn't mean "better" or "required for MSIX." Use the namespace
that owns the element or attribute you need, and verify that the minimum Windows
version for that feature matches the minimum OS that your MSIX package supports.
Namespace numbers can also skip; for example, the Learn namespace-additions page
lists `uap8`, `uap10`, `uap11`, `uap12`, `uap13`, and `uap15`, but not `uap9` or
`uap14`.

> [!CAUTION]
> Declaring and using a namespace whose features require a newer OS than your
> target OS can cause deployment or behavior issues on older devices. Test the
> MSIX package on the minimum supported OS, not only on the current development OS.

## Declare a namespace on the Package element

Declare every namespace prefix on the root `<Package>` element. The default
namespace is the foundation namespace and doesn't use a prefix. Additional schema
families use `xmlns:prefix` declarations.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  xmlns:desktop8="http://schemas.microsoft.com/appx/manifest/desktop/windows10/8"
  IgnorableNamespaces="uap uap10 desktop8">
```

After you declare a prefix, use that prefix only on elements or attributes that
belong to that namespace. For example, use a `desktop8:` element only where the
schema reference shows a `desktop8` element or attribute is allowed.

## Use IgnorableNamespaces correctly

The `IgnorableNamespaces` attribute is also declared on the root `<Package>`
element. It contains a space-separated list of namespace prefixes. The schema
reference for `<Package>` states that ignored namespace elements aren't validated
and should be considered untrusted.

`IgnorableNamespaces` helps older schema validators ignore markup that they don't
understand, but it doesn't make a feature work on an OS that doesn't implement the
feature. It also doesn't remove package identity, capability, extension, or Store
policy requirements. Treat it as a compatibility mechanism for markup, not as an
OS-version bypass.

## Namespace family reference

The following table gives a scoped map of the main package manifest namespace
families. Use it to find the right schema reference area; use the individual element
pages for exact syntax and element-specific requirements.

| Prefix or prefix family | Purpose | Schema reference |
| --- | --- | --- |
| default, `f`, `f2` | Foundation package structure, including root package elements such as `<Package>`, `<Identity>`, `<Properties>`, `<Resources>`, `<Dependencies>`, and `<Capabilities>`. | [`<Package>` element](/uwp/schemas/appxpackage/uapmanifestschema/element-f-package) and [element hierarchy](/uwp/schemas/appxpackage/uapmanifestschema/root-elements) |
| `uap`, `uap2`-`uap8`, `uap10`-`uap13`, `uap15` | Universal Windows app platform elements, capabilities, and app model extension points. Some later `uap` attributes also apply to packaged desktop apps; check the element page. | [Extensions in the package manifest schema](/uwp/schemas/appxpackage/uapmanifestschema/extensions) and [element hierarchy](/uwp/schemas/appxpackage/uapmanifestschema/root-elements) |
| `desktop`, `desktop2`-`desktop10` | Packaged desktop extension points and desktop-specific package manifest features. | [Extensions in the package manifest schema](/uwp/schemas/appxpackage/uapmanifestschema/extensions), [`desktop7:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop7-extension), and [`desktop8:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop8-extension) |
| `com`, `com2`-`com5` | COM extension registrations for packaged apps, including COM server and COM interface registrations. | [`com4:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-extension) and [`com5:InProcessHandler`](/uwp/schemas/appxpackage/uapmanifestschema/element-com5-inprocesshandler) |
| `rescap`, `rescap2`-`rescap6` | Restricted capability declarations. These capabilities can have Store approval, policy, or device requirements beyond schema validation. | [`rescap:Capability`](/uwp/schemas/appxpackage/uapmanifestschema/element-rescap-capability) and [`<Capabilities>` element](/uwp/schemas/appxpackage/uapmanifestschema/element-f-capabilities) |
| `holo`, `mobile`, `serverpreview` | Device-family or preview namespaces listed in the Windows 10 package manifest schema namespace history. | [What's different in Windows 10](/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10) |
| `cloudFiles`, `deployment`, `heap`, `printSupport`, `virtualization` | Feature-specific namespaces added by later Windows 10 or Windows 11 releases. | [What's different in Windows 10](/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10) |

## Verified namespace additions by Windows version

The following mappings come from the Learn page
[What's different in Windows 10](/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10),
which lists the namespaces and XML prefixes added in each Windows 10 or Windows 11
update. Use individual element pages to confirm any element-specific minimum OS
version before you depend on that element.

| Windows version or build | Prefixes added |
| --- | --- |
| Windows 10 version 1507, Build 10240 | `uap`, `f`, `holo`, `mobile`, `rescap`, `serverpreview` |
| Windows 10 version 1511, Build 10586 | `f2`, `uap2` |
| Windows 10 version 1607, Build 14393 | `desktop`, `rescap2`, `uap3` |
| Windows 10 version 1703, Build 15063 | `com`, `desktop2`, `rescap3`, `uap4` |
| Windows 10 version 1709, Build 16299.15 | `com2`, `desktop3`, `uap5` |
| Windows 10 version 1803, Build 17134 | `desktop4`, `rescap4`, `uap6` |
| Windows 10 version 1809, Build 17763 | `desktop5`, `rescap5`, `uap7` |
| Windows 10 version 1903, Build 18362 | `desktop6`, `rescap6`, `uap8` |
| Windows 10 version 2004, Build 19041 | `com3`, `printSupport`, `uap10` |
| Windows 10, Build 19645 | `cloudFiles`, `uap11` |
| Windows 10, Build 20348 | `com4`, `deployment`, `desktop7`, `uap12`, `virtualization` |
| Windows 11, Build 22000 | `com5`, `desktop8`, `heap`, `uap13` |
| Windows 11, Build 22159 | `desktop9` |
| Windows 11, Build 22621 | `desktop10`, `uap15` |

<!-- SME question: The Learn namespace-additions page lists desktop7 under Windows 10
Build 20348, but the desktop7:Extension requirements table currently says Minimum OS
Version Windows 10 (Build 19645). Which minimum should this overview use for
desktop7? -->

## Packaged desktop apps and UWP apps

Don't infer the app type from the prefix alone. The `uap` family includes UWP app
model features, but some later `uap` attributes are used by packaged desktop apps.
The `desktop` and `com` families are commonly used by packaged desktop apps that
need desktop integration or COM registration. The schema reference for the specific
element or attribute is the source of truth for whether it applies at the package
level, application level, or both.

For extension points, check whether the element is allowed under package-level
`<Extensions>` or under an application's `<Extensions>` element. A package can
contain more than one application, and application-level extensions apply only to
the application that declares them.

## Authoring checklist

Before you add a namespace to a package manifest, confirm the following items:

- The element or attribute you need is documented under the package manifest schema
  reference.
- The namespace prefix is declared on `<Package>` with the exact namespace URI from
  the schema reference.
- The prefix is included in `IgnorableNamespaces` when your compatibility target
  needs older validators to ignore that markup.
- The feature's minimum Windows version is at or below your minimum supported OS.
- The package has any required capabilities, policy approvals, or deployment
  conditions for the feature.
- The MSIX package has been tested on the minimum supported OS.
