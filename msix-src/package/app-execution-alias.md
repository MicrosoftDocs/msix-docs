---
title: App execution aliases in MSIX packages
description: Learn how to declare an app execution alias and inspect its target executable with PowerShell.
ms.date: 08/27/2026
ms.topic: how-to
keywords: msix, app execution alias, powershell, manifest
---

# App execution aliases in MSIX packages

An app execution alias lets a user start a packaged application from a command prompt, a script, or the **Run** dialog by entering a short name such as `contoso.exe`. Windows registers the alias during package registration and resolves it to the application declared in the package manifest.

Declare the `windows.appExecutionAlias` extension under the application that handles the alias. The extension's optional `Executable` attribute can override the executable declared by the containing `Application`. Each `ExecutionAlias` child specifies an alias that activates that application by using the extension executable when present, otherwise the application executable.

```xml
<Application
    Id="ContosoApp"
    Executable="Contoso\Contoso.exe"
    EntryPoint="Windows.FullTrustApplication">
  <Extensions>
    <uap5:Extension
        Category="windows.appExecutionAlias"
        Executable="Contoso\Contoso.exe"
        EntryPoint="Windows.FullTrustApplication">
      <uap5:AppExecutionAlias>
        <uap5:ExecutionAlias Alias="contoso.exe" />
      </uap5:AppExecutionAlias>
    </uap5:Extension>
  </Extensions>
</Application>
```

For schema requirements and supported attributes, see [`uap5:AppExecutionAlias`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) and [`uap5:ExecutionAlias`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-executionalias).

## Inspect aliases with PowerShell

The Appx PowerShell module doesn't provide a dedicated cmdlet that returns an alias-to-executable mapping. Use [`Get-AppxPackage`](/powershell/module/appx/get-appxpackage) to select an installed package and pipe it to [`Get-AppxPackageManifest`](/powershell/module/appx/get-appxpackagemanifest). The returned XML document contains the registered declarations.

The following example lists each alias and the executable declared by its parent extension. It uses namespace-independent XPath so it works with app execution alias schema versions such as `uap3` and `uap5`.

```powershell
$manifest = Get-AppxPackage -Name "Contoso.Reader" |
    Get-AppxPackageManifest

$extensions = $manifest.SelectNodes(
    "//*[local-name()='Extension' and @Category='windows.appExecutionAlias']")

foreach ($extension in $extensions) {
    $application = $extension.SelectSingleNode(
        "ancestor::*[local-name()='Application'][1]")
    $executable = if ($extension.HasAttribute("Executable")) {
        $extension.GetAttribute("Executable")
    } else {
        $application.GetAttribute("Executable")
    }

    foreach ($alias in $extension.SelectNodes(
        "./*[local-name()='AppExecutionAlias']/*[local-name()='ExecutionAlias']")) {
        [pscustomobject]@{
            Alias         = $alias.GetAttribute("Alias")
            ApplicationId = $application.GetAttribute("Id")
            Executable    = $executable
        }
    }
}
```

`Get-AppxPackageManifest` reads the manifest of a package registered for the current user. To inspect a package registered for another user, specify the `-User` parameter and run PowerShell with administrator privileges.

Don't use the versioned package installation path as a stable launch target. The path changes when the package version or architecture changes. Use the registered alias when you need a stable command-line launch name.

An execution alias is available only while the package is registered for that user and the alias is enabled. Users can enable or disable aliases in **Manage app execution aliases**, and Windows can require a choice when packages declare conflicting alias names.
