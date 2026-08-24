---
description: Learn how IT administrators create desktop shortcuts for installed MSIX applications by using an Application User Model ID, either manually in File Explorer or with a PowerShell script.
title: Create desktop shortcuts for MSIX applications
ms.date: 08/24/2026
ms.topic: how-to
keywords: windows 10, windows 11, msix, shortcut, desktop, AUMID, IT pro, deployment
---

# Create desktop shortcuts for MSIX applications

Users often expect a desktop shortcut for the applications you deploy. An MSIX package can
register one or more applications with the Windows shell so users can find them in the Start menu,
but package registration doesn't create desktop shortcuts for those applications.

This article describes how to create desktop shortcuts for applications that an MSIX package has
already registered on a device, including packaged desktop apps and UWP apps. It's written for IT
administrators who deploy packages they didn't author. If you're the package author and can change
the package, see [Alternatives you can build into the package](#alternatives-you-can-build-into-the-package).

## Why you can't rely on the package executable path

A shortcut to a packaged application should activate the application through its
[Application User Model ID (AUMID)](/windows/win32/shell/appids) rather than through a file path.
Pointing a shortcut directly at the executable inside the package install folder is brittle for
several reasons:

- **The install path contains the package version and architecture.** A package installs to a folder
  such as `C:\Program Files\WindowsApps\Contoso.ExpenseApp_1.0.0.0_x64__8wekyb3d8bbwe`. Every
  package update changes the version segment, so any shortcut that hardcodes the path stops working
  after the next update.
- **The executable path doesn't identify the registered application.** A package can register
  multiple applications. The package executable path alone doesn't tell the shell which registered
  application entry point, display name, tile, or activation behavior you intend to use.
- **There's no Start menu shortcut to copy.** Applications from an MSIX package aren't represented
  as `.lnk` files under `%ProgramData%\Microsoft\Windows\Start Menu\Programs` or the equivalent
  per-user folder, so there's nothing there to copy to the desktop.

Instead, target the virtual `shell:AppsFolder` shell folder, which exposes registered applications
on the device by AUMID.

## Find the AUMID

An AUMID for an application in an MSIX package has the form
`<PackageFamilyName>!<ApplicationId>`, for example
`Contoso.ExpenseApp_8wekyb3d8bbwe!App`. `ApplicationId` is the `Id` attribute of the
`<Application>` element in the package manifest.

A single MSIX package can register many applications, each with its own AUMID, so confirm you have
the AUMID of the specific application you want rather than assuming the package contains only one.

### Use Get-StartApps

The [`Get-StartApps`](/powershell/module/startlayout/get-startapps) cmdlet lists the display name
and AUMID of every application in the current user's Start menu:

```powershell
Get-StartApps | Where-Object { $_.AppID -like '*Contoso*' }
```

`Get-StartApps` returns entries for both packaged and unpackaged applications. Packaged applications
have an AUMID containing `!`; unpackaged applications have a different identifier form. It only
returns applications that appear in the Start menu. For example, it omits an application whose
manifest sets `AppListEntry="none"` on its `<uap:VisualElements>` element.

### Enumerate every registered application in a package

To list every application registered from a package for the account you inspect, including
applications hidden from the Start menu, read the package manifest:

```powershell
Get-AppxPackage -Name Contoso.ExpenseApp | ForEach-Object {
    $familyName = $_.PackageFamilyName
    (Get-AppxPackageManifest $_).Package.Applications.Application.Id | ForEach-Object {
        "$familyName!$_"
    }
}
```

Add `-User <username>` or `-AllUsers` to `Get-AppxPackage` to inspect packages registered for other
accounts. Both parameters require an elevated PowerShell session.

### Browse the Applications folder

To see the same list in the UI, press **Win+R**, enter `shell:AppsFolder`, and select **OK**. To
show the AUMID for each entry, switch the view to **Details**, then right-click the column headings
and add the **AppUserModelId** column.

## Create a shortcut manually

For a one-off shortcut on a single device:

1. Press **Win+R**, enter `shell:AppsFolder`, and select **OK**.
1. Locate the application.
1. Right-click it and select **Create shortcut**, or drag it onto the desktop.

Windows creates the shortcut with the application's registered icon and display name, so this method
requires no extra icon work. It applies only to the user account you're signed in as.

## Create a shortcut with PowerShell

To create shortcuts in a script, target `explorer.exe` and pass the `shell:AppsFolder` path as an
argument. Explorer resolves the AUMID and activates the application.

```powershell
$aumid    = 'Contoso.ExpenseApp_8wekyb3d8bbwe!App'
$linkPath = Join-Path ([Environment]::GetFolderPath('Desktop')) 'Contoso Expenses.lnk'

$shell             = New-Object -ComObject WScript.Shell
$shortcut          = $shell.CreateShortcut($linkPath)
$shortcut.TargetPath = "$env:WINDIR\explorer.exe"
$shortcut.Arguments  = "shell:AppsFolder\$aumid"
$shortcut.Save()
```

> [!IMPORTANT]
> Set `TargetPath` to `explorer.exe` and put the `shell:AppsFolder` path in `Arguments`. If you
> assign `shell:AppsFolder\<AUMID>` to `TargetPath` instead, `WScript.Shell` discards the value and
> saves a shortcut with an empty target. The call doesn't raise an error, so the shortcut appears to
> be created successfully but does nothing when the user selects it.

`WScript.Shell` doesn't validate `TargetPath`, `Arguments`, or `IconLocation` when it saves. A
shortcut that references a misspelled AUMID is created without error and fails silently at launch,
so validate the AUMID before you create the shortcut.

### A reusable script

The following function validates the AUMID against the Applications folder before it writes the
shortcut, and supports both per-user and all-users placement:

```powershell
function New-MsixAppShortcut {
    [CmdletBinding()]
    param(
        # AUMID in the form <PackageFamilyName>!<ApplicationId>.
        [Parameter(Mandatory)]
        [string]$Aumid,

        # Shortcut file name, without the .lnk extension.
        [Parameter(Mandatory)]
        [string]$Name,

        # Create the shortcut on the all-users desktop instead of the current user's desktop.
        # Requires an elevated session.
        [switch]$AllUsers,

        # Optional .ico, .exe, or .dll to use for the shortcut icon.
        [string]$IconPath
    )

    $appsFolder = (New-Object -ComObject Shell.Application).NameSpace(
        'shell:::{4234d49b-0245-4df3-b780-3893943456e1}')

    if (-not ($appsFolder.Items() | Where-Object { $_.Path -eq $Aumid })) {
        throw "No application is registered with the AUMID '$Aumid' for the current user."
    }

    $desktop = if ($AllUsers) {
        Join-Path $env:PUBLIC 'Desktop'
    }
    else {
        [Environment]::GetFolderPath('Desktop')
    }

    $linkPath            = Join-Path $desktop "$Name.lnk"
    $shell               = New-Object -ComObject WScript.Shell
    $shortcut            = $shell.CreateShortcut($linkPath)
    $shortcut.TargetPath = "$env:WINDIR\explorer.exe"
    $shortcut.Arguments  = "shell:AppsFolder\$Aumid"
    $shortcut.Description = $Name

    if ($IconPath) {
        $shortcut.IconLocation = "$IconPath,0"
    }

    $shortcut.Save()
    Write-Output $linkPath
}
```

Call it with the AUMID you found earlier:

```powershell
New-MsixAppShortcut -Aumid 'Contoso.ExpenseApp_8wekyb3d8bbwe!App' -Name 'Contoso Expenses'
```

The validation step checks the Applications folder for the user running the script. Run it in the
user's own session, not as SYSTEM, or the check fails even when the AUMID is correct.

## Set the shortcut icon

A shortcut created by the manual File Explorer method already uses the application's registered
icon. A shortcut created with `WScript.Shell` shows the File Explorer icon unless you set
`IconLocation`.

Two constraints apply when you choose an icon:

- **Use a stable icon source.** Common shortcut icon sources are `.ico`, `.exe`, and `.dll` files.
  Although the package manifest references image assets such as `Square44x44Logo` and
  `Square150x150Logo`, those assets usually live under the package install folder.
- **Don't reference the package install folder.** That path contains the package version and changes
  with every update, which can leave the shortcut with a missing icon.

Stage an `.ico` in a stable location that you control, such as `%ProgramData%\Contoso\Icons`, and
point `IconLocation` there. If you don't need a custom icon, prefer the manual method or accept the
default icon.

## Deploy shortcuts at scale

Consider the following when you roll shortcuts out across a fleet.

### Choose per-user or all-users

| Location | Path | Notes |
| --- | --- | --- |
| Current user's desktop | `[Environment]::GetFolderPath('Desktop')` | Honors folder redirection and OneDrive Known Folder Move. |
| All-users desktop | `%PUBLIC%\Desktop` | Requires administrator rights. The shortcut appears for every user who signs in to the device. |

An all-users shortcut is visible to users who don't have the package registered. Selecting it does
nothing for those users, so prefer a per-user shortcut when the package isn't registered for
everyone on the device.

### Account for registration timing

The AUMID resolves only after the package is registered for the user who is signed in. If you
provision a package for all users, Windows completes per-user registration at that user's next sign
in. A shortcut script that runs before registration finishes fails its validation check.

Run shortcut creation in the user context after sign-in, for example from a logon script, an Intune
user-targeted script, or a Configuration Manager deployment configured to run with user rights. For
more information about deploying the package itself, see
[Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md).

### Plan for removal

Removing the MSIX package doesn't delete shortcuts you created. Remove them in the same script or
task that removes the package, or users are left with shortcuts that do nothing.

## Alternatives you can build into the package

If you control the package, you have options that don't require a separate shortcut script:

- **Declare an app execution alias.** An alias creates a stable, version-independent command name
  that resolves through `%LOCALAPPDATA%\Microsoft\WindowsApps`, which is on the user's `PATH`. A
  shortcut can then target the alias directly. See
  [Start your application by using an alias](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias).
- **Create the shortcut from a package script.** The Package Support Framework can run a script the
  first time the application starts. See
  [Create an application shortcut by running a script using Package Support Framework](../psf/create-shortcut-with-script-package-support-framework.md).

Create desktop shortcuts only where they're genuinely useful. Applications from an MSIX package are
already discoverable in the Start menu and in search.

## Troubleshooting

| Symptom | Cause | Resolution |
| --- | --- | --- |
| Selecting the shortcut does nothing. | The AUMID is misspelled, or the package isn't registered for the signed-in user. | Confirm the AUMID appears in `Get-StartApps` or `shell:AppsFolder` in that user's session. |
| The shortcut worked, then stopped after an update. | The shortcut targets the package install path instead of the AUMID. | Recreate it using `explorer.exe` and `shell:AppsFolder\<AUMID>`. |
| The shortcut has an empty target. | `shell:AppsFolder\<AUMID>` was assigned to `TargetPath`. | Set `TargetPath` to `explorer.exe` and put the `shell:AppsFolder` path in `Arguments`. |
| The shortcut shows the File Explorer icon. | `IconLocation` isn't set. | Set `IconLocation` to a staged `.ico`, or create the shortcut from `shell:AppsFolder` in File Explorer. |
| `Get-StartApps` doesn't list the application. | The application is hidden from the Start menu. | Enumerate the package manifest instead. See [Enumerate every registered application in a package](#enumerate-every-registered-application-in-a-package). |
| The application isn't in `shell:AppsFolder` for one user. | The package is provisioned but not yet registered for that user. | Sign the user in to complete registration, then create the shortcut. |

## Related articles

- [Managing MSIX with PowerShell](powershell-msix-cmdlets.md)
- [Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md)
- [Find the Application User Model ID of an installed app](/windows/configuration/store/find-aumid)
- [Application User Model IDs](/windows/win32/shell/appids)
