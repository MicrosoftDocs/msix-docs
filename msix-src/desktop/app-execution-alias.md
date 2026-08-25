---
description: Learn how to declare and use an app execution alias for a packaged desktop application.
title: Start an MSIX application with an app execution alias
ms.date: 08/25/2026
ms.topic: how-to
keywords: windows 10, windows 11, uwp, msix
ms.custom: RS5
---

# Start an MSIX application with an app execution alias

An app execution alias is a command name that Windows registers for a packaged
application. Windows manages the alias under the user's profile in
`%LOCALAPPDATA%\Microsoft\WindowsApps`, a directory that is on the user's `PATH`.
When the user starts the alias, Windows activates the packaged application with
package identity.

Use an app execution alias when you need a stable command name for scripts,
shortcuts, jump lists, documentation, or support steps. The alias avoids taking a
dependency on the MSIX package installation path, such as a version-stamped path
under `C:\Program Files\WindowsApps`. That path can change when the MSIX package
is updated.

## Declare an alias for a packaged desktop application

For a packaged desktop application, declare the alias in an app execution alias
manifest extension. The extension category is `windows.appExecutionAlias`. The
extension uses the `uap3:AppExecutionAlias` element and a
`desktop:ExecutionAlias` child element.

The following manifest excerpt declares `contoso.exe` as the alias for a packaged
desktop application executable that's included in the MSIX package at
`Contoso\Contoso.exe`.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap uap3 desktop">
  <Applications>
    <Application
      Id="Contoso"
      Executable="Contoso\Contoso.exe"
      EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements
        DisplayName="Contoso"
        Description="Contoso"
        BackgroundColor="transparent"
        Square150x150Logo="Assets\Logo.png"
        Square44x44Logo="Assets\SmallLogo.png" />
      <Extensions>
        <uap3:Extension
          Category="windows.appExecutionAlias"
          Executable="Contoso\Contoso.exe"
          EntryPoint="Windows.FullTrustApplication">
          <uap3:AppExecutionAlias>
            <desktop:ExecutionAlias Alias="contoso.exe" />
          </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Use these rules when you adapt the example:

- Set `uap3:Extension/@Category` to `windows.appExecutionAlias`.
- Set `uap3:Extension/@Executable` to a package-relative path for the executable
  to start. The executable must be included in the MSIX package.
- Set `uap3:Extension/@EntryPoint` to `Windows.FullTrustApplication` for a
  packaged desktop application.
- Set `desktop:ExecutionAlias/@Alias` to the command name. The alias must end
  with `.exe` and must follow the schema restrictions for invalid characters.
- Include the `uap3` and `desktop` namespaces in `IgnorableNamespaces`.

The `uap3:Extension` and `uap3:AppExecutionAlias` schema pages list Windows 10,
version 1607 (build 14393) as the minimum OS version. Some newer attributes for
app execution aliases are available only in later schema namespaces.

## UWP application variant

For a UWP application, use the `uap5` app execution alias elements instead of the
packaged desktop `desktop:ExecutionAlias` element. Place the following fragment
under the UWP application's `Application` element. Declare the `uap5` namespace
on the `Package` element and include `uap5` in `IgnorableNamespaces`.

```xml
<Extensions>
  <uap5:Extension Category="windows.appExecutionAlias">
    <uap5:AppExecutionAlias>
      <uap5:ExecutionAlias Alias="contosouwp.exe" />
    </uap5:AppExecutionAlias>
  </uap5:Extension>
</Extensions>
```

The `uap5:Extension`, `uap5:AppExecutionAlias`, and `uap5:ExecutionAlias` schema
pages list Windows 10, version 1709 (build 16299) as the minimum OS version.

## Invoke the alias

After the MSIX package is installed for the user, start the packaged application
by using the alias name.

| Location | Example |
| --- | --- |
| **Run** dialog | Press **Win+R**, enter `contoso.exe`, and select **OK**. |
| Command Prompt | Run `contoso.exe` or `contoso.exe --help`. |
| PowerShell | Run `contoso.exe` or `& contoso.exe --help`. |

If you use MSIX Core on Windows 10, version 1703 or earlier, app execution
aliases work from **Win+R** but not from Command Prompt or PowerShell. For more
information, see [MSIX Core](../msix-core/msixcore.md).

## Practical notes

- Choose a unique, product-specific alias. App execution aliases share the user's
  command namespace, so generic names are more likely to collide.
- Users can search for **Manage app execution aliases** in Windows Search or
  Settings to view and manage registered aliases.
- The alias is per user because the `WindowsApps` location is under the user's
  profile and is on the user's `PATH`.
- Don't point scripts or shortcuts to a version-stamped path under
  `C:\Program Files\WindowsApps`. Use the alias or another supported activation
  path instead.
- If you create shortcuts or jump list entries that target the alias, install the
  MSIX package before you create or validate those links.
- App execution aliases are removed when the MSIX package is uninstalled.

## Reference

- [Integrate your desktop app with Windows by using packaging extensions](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-in-different-ways)
- [uap3:Extension schema](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-extension)
- [uap3:AppExecutionAlias schema](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-appexecutionalias)
- [desktop:ExecutionAlias schema](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop-executionalias)
- [uap5:Extension schema](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-extension)
- [uap5:AppExecutionAlias schema](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)
- [uap5:ExecutionAlias schema](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-executionalias)
