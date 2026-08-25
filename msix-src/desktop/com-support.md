---
description: Learn how MSIX packages register COM servers and type libraries.
title: COM support in MSIX packages
ms.date: 08/25/2026
ms.topic: concept-article
keywords: windows 10, windows 11, msix, com
---

# COM support in MSIX packages

An MSIX package can declare COM registrations in its package manifest. Windows uses those
manifest extensions to make the COM registrations available at package deployment time instead
of requiring the installer or the packaged application to write directly to `HKCR` or `HKLM`.
The registration is serviced with the package and is removed when the package is uninstalled.

Use this article when you need to package an existing desktop application that exposes COM
classes, COM servers, interfaces, proxy stubs, or type libraries.

## How packaged COM registration works

COM registrations in an MSIX package use package manifest extensions:

- A `windows.comServer` extension registers COM servers and class registrations. With the
  current `com4` schema, a `com4:ComServer` extension can contain out-of-process servers,
  in-process servers, service servers, surrogate servers, class registrations, ProgIDs, and
  `TreatAsClass` registrations.
- A `windows.comInterface` extension registers interface-related data. With the current
  `com4` schema, a `com4:ComInterface` extension can contain `com4:Interface`,
  `com4:ProxyStub`, and `com4:TypeLib` registrations.
- A `com4:TypeLib` under `com4:ComInterface` defines the type library and one or more
  `com4:Version` children. The `com4` schema allows multiple `com4:Version` children
  under the same type library ID. Each `com4:Version` must specify `com4:Win32Path`,
  `com4:Win64Path`, or both.
- A `com4:TypeLib` under `com4:Class` or `com4:Interface` associates a class or interface
  with a type library by referencing the `Id` of a `com4:TypeLib` definition. If it includes
  `VersionNumber`, that value must reference a `com4:Version` under the referenced type
  library.

The `com4` schema is a superset and replacement for the older `com`, `com2`, and `com3`
syntax. Use `com4` for new Windows 11 package manifests that can depend on Windows 10
Build 20348 or later. If your package must install on earlier Windows versions, review the
older namespace schema and validate the manifest against every target OS version.

> [!IMPORTANT]
> Do not treat packaged COM as direct registry authoring. Packaged COM support works with
> existing COM activation APIs, but application extensions that directly read COM registry
> keys might not work because packaged COM data is stored in a private location.

## Server and activation scenarios

The following table summarizes common packaging decisions.

| Scenario | Manifest registration | Notes |
| --- | --- | --- |
| Out-of-process COM server in the package | `com4:ExeServer` under `com4:ComServer` | The executable path is relative to the package root. COM servers registered in the manifest use Activate As Package behavior, so the server runs with package and application claims. |
| In-process COM DLL in the package | `com4:InProcessServer` under `com4:ComServer` | Use this only after validating the client and package scenario. In-process modules loaded into processes that aren't in the package, such as shell extensions, aren't supported. |
| Same CLSID with in-process and out-of-process registrations | Top-level `com4:Class` with server `com4:ClassReference` children | The `com4` schema supports this structure so that one class registration can be shared by multiple server registrations. |
| Interface, proxy stub, or type library | `com4:ComInterface` | Keep dependent registrations in a consistent `com4` schema. For example, an interface that uses the OLE Universal Marshaler must include a `com4:TypeLib` reference. |
| Type library for 32-bit and 64-bit clients | `com4:TypeLib` with `com4:Version`, `com4:Win32Path`, and `com4:Win64Path` | Include paths for the architectures you support. The paths are relative to the package root and must reference files in the package. |

In multi-application packages, put COM server registrations under the correct
`Applications/Application` element. Out-of-process COM server processes run with the identity
of the ancestor `Application` element.

## Example: in-process server, class, interface, and type library

The following fragment shows the COM-related manifest extensions for one packaged desktop
application. Replace the GUIDs, paths, display names, ProgIDs, and interface details with
values from your COM server.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  xmlns:com4="http://schemas.microsoft.com/appx/manifest/com/windows10/4"
  IgnorableNamespaces="uap10 com4">
  <Applications>
    <Application
      Id="ContosoApp"
      Executable="ContosoApp.exe"
      EntryPoint="Windows.FullTrustApplication"
      uap10:RuntimeBehavior="packagedClassicApp"
      uap10:TrustLevel="mediumIL">
      <Extensions>
        <com4:Extension Category="windows.comServer">
          <com4:ComServer>
            <com4:InProcessServer Path="Contoso.Component.dll">
              <com4:Class
                Id="11111111-1111-1111-1111-111111111111"
                DisplayName="Contoso Component Class"
                ProgId="Contoso.Component.1"
                VersionIndependentProgId="Contoso.Component"
                ThreadingModel="Both">
                <com4:TypeLib
                  Id="22222222-2222-2222-2222-222222222222"
                  VersionNumber="1.0" />
              </com4:Class>
            </com4:InProcessServer>
          </com4:ComServer>
        </com4:Extension>

        <com4:Extension Category="windows.comInterface">
          <com4:ComInterface>
            <com4:TypeLib Id="22222222-2222-2222-2222-222222222222">
              <com4:Version VersionNumber="1.0" DisplayName="Contoso Type Library">
                <com4:Win64Path Path="Contoso.Component.tlb" />
              </com4:Version>
            </com4:TypeLib>
            <com4:Interface
              Id="33333333-3333-3333-3333-333333333333"
              UseUniversalMarshaler="true">
              <com4:TypeLib
                Id="22222222-2222-2222-2222-222222222222"
                VersionNumber="1.0" />
            </com4:Interface>
          </com4:ComInterface>
        </com4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

This example defines a 64-bit type library path. If your package supports 32-bit COM clients,
add the corresponding `com4:Win32Path` entry and include the 32-bit type library file in the
package. If your in-process DLL path differs by architecture, use the architecture-specific
`com4:InProcessServerDll` child elements instead of a single `Path` value.

## Limitations and operational risks

- In-process extensions loaded into processes that aren't in the MSIX package aren't
  supported. This boundary affects shell extensions and similar plug-in models where an
  external process loads the package's DLL.
- The `com4` schema page notes that in-process COM support is currently functionally limited
  and intended for packages with external location because normal package install locations
  can prevent DLLs from being loaded outside the package. Test this scenario on every target
  OS and package layout.
- Out-of-process COM servers registered in the manifest use Activate As Package behavior.
  Other COM activation behaviors, such as `RunAs`, aren't supported for manifest-registered
  COM servers.
- Some extension visibility settings require restricted capabilities. For example,
  `desktop7:CompatMode="classic"` and `desktop7:Scope="machine"` have capability
  requirements. Don't add those settings unless your package is approved for the required
  capability.
- If a COM client or plug-in installer directly reads or writes classic COM registry keys,
  validate the scenario. Packaged COM registration is not the same as writing classic registry
  keys under `HKCR` or `HKLM`.

## Related schema and conceptual references

- [com4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-extension)
- [com4:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-comserver)
- [com4:InProcessServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-inprocessserver)
- [com4:ExeServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-exeserver)
- [com4:ComInterface](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-cominterface)
- [com4:TypeLib in ComInterface](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-cominterface-typelib)
- [com4:TypeLib in Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-class-typelib)
- [com4:Version](/uwp/schemas/appxpackage/uapmanifestschema/element-com4-version)
- [Prepare to package a desktop application](desktop-to-uwp-prepare.md)
- [Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md)
- [Component Object Model (COM)](/windows/win32/com/component-object-model--com--portal)

<!-- SME question: Should this article include a tested example for multiple com4:Version
children under one com4:TypeLib, including the expected behavior when a class references one
VersionNumber? -->
