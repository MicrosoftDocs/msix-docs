---
title: Understanding how packaged desktop apps run on Windows
description: This topic provides a deep dive into how the OS behaves with packaged desktop apps.
ms.date: 01/18/2022
ms.topic: article
keywords: windows 11, windows 10, uwp, msix
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
---

# Understanding how packaged desktop apps run on Windows

This topic describes the types of desktop apps that you can create a Windows app package for, together with some operating system (OS) behaviors&mdash;and other specifics&mdash;that are important to be aware of. We'll go into details of the following items (as we'll see, the specific behavior depends on the type of your app):

* Your app's [install location](#installation) and [working directory](#working-directory-and-application-files) (which might be different from what your app has assumed in the past).
* The OS's [file system](#file-system) and [registry](#registry) behavior.
* [Uninstallation](#uninstallation).

## Types of desktop app

There are two types of desktop app that you can create and package. You declare your app's type in its app package manifest by using the **uap10:RuntimeBehavior** attribute of the [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) element:

* One type includes both WinUI 3 apps (which use the [Windows App SDK](/windows/apps/windows-app-sdk/)) and Desktop Bridge apps (Centennial). Declared with `uap10:RuntimeBehavior="packagedClassicApp"`.
* The other type represents other kinds of Win32 app, including apps [packaged with external location](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps). Declared with `uap10:RuntimeBehavior="win32App"`.

Universal Windows Platform (UWP) apps (`uap10:RuntimeBehavior="windowsApp"`) are also packaged; but this topic isn't about them.

And then the **uap10:TrustLevel** attribute (of the same **Application** element) determines whether or not your packaged app's process runs inside an app container.

* A *full trust* app. Declared with `uap10:TrustLevel="mediumIL"`.
* An app that runs in a lightweight app container (and is therefore isolated using file system and registry virtualization). Declared with `uap10:TrustLevel="appContainer"`.

> [!IMPORTANT]
> For more details, dependencies, and capability requirements, see the documentation for those two attributes in [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application). Also see [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041).

## The purpose of packaging, and app containers

The purpose of packaging your app is to grant it *package identity* at runtime. Package identity is needed for certain Windows features (see [Features that require package identity](/windows/apps/desktop/modernize/modernize-packaged-apps)). You can package *all* combinations of app types described above (and thereby benefit from *package identity*).

But a key goal of an `appContainer` app is to separate app state from system state as much as possible, while maintaining compatibility with other apps. Windows accomplishes that by detecting and redirecting certain changes that it makes to the file system and registry at runtime (known as *virtualizing*). We'll call out when a section applies only to virtualized apps.

## Installation

App packages are installed on a per-user basis instead of system-wide. The default location for new packages on a new machine is under `C:\Program Files\WindowsApps\<package_full_name>`, with the executable named *app_name.exe*. But packages can be installed in other places; for example, Visual Studio's **Start** commands use the project's `$(OutDir)`.

After deployment, package files are marked read-only, and are heavily locked down by the operating system (OS). Windows prevents apps from launching if those files are tampered with.

## File system

The OS supports different levels of file system operations for packaged desktop apps, depending on the folder location.

### Optimized for your device

In order to avoid duplication of files (to optimize for disk storage space and reduce the bandwidth needed when downloading files), the OS leverages single storage and hard linking of files. When a user downloads an MSIX package, the `AppxManifest.xml` is used to determine whether the data contained with the package already exist on disk from an earlier package installation. If the same file exists in multiple MSIX packages, then the OS stores the shared file on disk once only, and creates hard links from both packages to the shared file. Since files are downloaded in 64Kb blocks, even if a percentage of a file being downloaded exists on disk, only the increment that's different is downloaded. That reduces the bandwidth used for downloading.

### AppData operations on Windows 10, version 1903 and later

This section applies only to virtualized apps.

All newly created files and folders in the user's `AppData` folder (for example, `C:\Users\<user_name>\AppData`) are written to a private per-user, per-app location; but merged at runtime to appear in the real `AppData` location. That allows some degree of state separation for artifacts that are used only by the app itself; which enables the system to clean up those files when the app is uninstalled.

Modifications to existing files under the user's `AppData` folder is allowed in order to provide a higher degree of compatibility and interactivity between apps and the OS. That reduces system "rot" because the OS is aware of every file or directory change made by an app. State separation also allows packaged desktop apps to pick up where an unpackaged version of the same app left off. Note that the OS doesn't support a virtual file system (VFS) folder for the user's `AppData` folder.

### AppData operations on OSes earlier than Windows 10, version 1903

This section applies only to virtualized apps.

All writes to the user's `AppData` folder (for example, `C:\Users\<user_name>\AppData`)&mdash;including create, delete, and update&mdash;are copied on write to a private per-user, per-app location. That creates the illusion that the packaged app is editing the real `AppData` when it's actually modifying a private copy. By redirecting writes that way, the system can track all file modifications made by the app. That allows the system to clean up those files when the app is uninstalled, thus reducing system "rot", and providing a better app removal experience for the user.

### Working directory, and application files

This section applies only to virtualized apps.

In addition to redirecting `AppData`, Windows' well-known folders (`System32`, `Program Files (x86)`, etc.) are dynamically merged with corresponding directories in the app package. Each package contains a folder named `VFS` at its root. Any reads of directories or files in the `VFS` directory are merged at runtime with their respective native counterparts. For example, an app could contain `C:\Program Files\WindowsApps\<package_full_name>\VFS\SystemX86\vc10.dll` as part of its app package, but the file would appear to be installed at `C:\Windows\System32\vc10.dll`. That maintains compatibility with desktop apps that expect files to live in non-package locations.

Writes to files/folders in the app package aren't allowed. Writes to files and folders that aren't part of the package are ignored by the OS, and are allowed as long as the user has permission.

### Common file system operations

This short reference table shows common file system operations and how the OS handles them.

| Operation | Result | Example |
| --- | --- | --- |
| Read or enumerate a well-known Windows file or folder | A dynamic merge of `C:\Program Files\<package_full_name>\VFS\<well_known_folder>` with the local system counterpart. | Reading `C:\Windows\System32` returns the contents of `C:\Windows\System32` plus the contents of `C:\Program Files\WindowsApps\<package_full_name>\VFS\SystemX86`. |
| Write under `AppData` | **Windows 10, version 1903 and later**: New files and folders created under the following directories are redirected to a per-user, per-package private location:<ul><li>Local</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Menu\Programs</li></ul>In response to a file open command, the OS will open the file from the per-user, per-package location first. If that location doesn't exist, then the OS will attempt to open the file from the real `AppData` location. If the file is opened from the real `AppData` location, then no virtualization for that file occurs. File deletes under `AppData` are allowed if user has permissions.<p/><p/>**Earlier than Windows 10, version 1903**: Copy on write to a per-user, per-app location. | `AppData` is typically `C:\Users\<user_name>\AppData`. |
| Write inside the package | Not allowed. The package is read-only. | Writes under `C:\Program Files\WindowsApps\<package_full_name>` aren't allowed. |
| Write outside the package | Allowed if the user has permissions. | A write to `C:\Windows\System32\foo.dll` is allowed if the package doesn't contain `C:\Program Files\WindowsApps\<package_full_name>\VFS\SystemX86\foo.dll`, and the user has permissions. |

### Packaged VFS locations

This section applies only to virtualized apps.

This table shows where files shipping as part of your package are overlaid on the system for the app. Your app will perceive these files to be in the listed system locations when in fact they're in the redirected locations inside `C:\Program Files\WindowsApps\<package_full_name>\VFS`. The *FOLDERID* locations are from the [**KNOWNFOLDERID**](/windows/win32/shell/knownfolderid) constants.

| System location | Redirected location (Under \[<package_root>\]\VFS\) | Valid on architectures |
 :--- | :--- | :---
*FOLDERID_SystemX86* | `SystemX86` | x86, amd64
*FOLDERID_System* | `SystemX64` | amd64
*FOLDERID_ProgramFilesX86* | `ProgramFilesX86` | x86, amd6
*FOLDERID_ProgramFilesX64* | `ProgramFilesX64` | amd64
*FOLDERID_ProgramFilesCommonX86* | `ProgramFilesCommonX86` | x86, amd64
*FOLDERID_ProgramFilesCommonX64* | `ProgramFilesCommonX64` | amd64
*FOLDERID_Windows* | `Windows` | x86, amd64
*FOLDERID_ProgramData* | Common `AppData` | x86, amd64
*FOLDERID_System*\catroot | `AppVSystem32Catroot` | x86, amd64
*FOLDERID_System*\catroot2 | `AppVSystem32Catroot2` | x86, amd64
*FOLDERID_System*\drivers\etc | `AppVSystem32DriversEtc` | x86, amd64
*FOLDERID_System*\driverstore | `AppVSystem32Driverstore` | x86, amd64
*FOLDERID_System*\logfiles | `AppVSystem32Logfiles` | x86, amd64
*FOLDERID_System*\spool | `AppVSystem32Spool` | x86, amd64

## Registry

This section (and its sub-sections) applies only to virtualized apps.

App packages contain a `registry.dat` file, which serves as the logical (virtual) equivalent of *HKLM\Software* in the real registry. At runtime, the virtual registry merges the contents of that hive into the native system hive to provide a single view of both. For example, if `registry.dat` contains a single key *Foo*, then a read of *HKLM\Software* at runtime will also appear to contain *Foo* (in addition to all the native system keys).

Although MSIX packages include *HKLM* and *HKCU* keys, they are treated differently. Only keys under *HKLM\Software* are part of the package; keys under *HKCU* or other parts of the registry are not. Writes to keys or values in the package aren't allowed. Writes to keys or values not part of the package are allowed as long as the user has permission.

All writes under *HKCU* are copied on write to a private per-user, per-app location. Traditionally, uninstallers are unable to clean *HKEY_CURRENT_USER* because the registry data for logged-out users is unmounted and unavailable.

All writes are kept during package upgrade, and deleted only when the app is removed entirely.

### Common registry operations

Most of this section applies only to virtualized apps.

This short reference table shows common registry operations and how the OS handles them.

| Operation | Result | Example |
| :--- | :--- | :--- |
| Read or enumerate *HKLM\Software* | A dynamic merge of the package hive with the local system counterpart. | If `registry.dat` contains a single key *Foo*, then at runtime a read of *HKLM\Software* shows the contents of both *HKLM\Software* and *HKLM\Software\Foo*. |
| Writes under *HKCU* | Copied on write to a per-user, per-app private location. | The same as `AppData` for files. |
| Writes inside the package. | Not allowed. The package is read-only. | Writes under *HKLM\Software* aren't allowed if a corresponding key/value exist in the package hive. |
| Writes outside the package | Ignored by the OS. Allowed if the user has permissions. | Writes under *HKLM\Software* are allowed as long as a corresponding key/value doesn't exist in the package hive, and the user has the correct access permissions. |

## Uninstallation

This section applies only to virtualized apps.

When a package is uninstalled by the user, all files and folders located under `C:\Program Files\WindowsApps\<package_full_name>` are removed, as well as any redirected writes to `AppData` or the registry that were captured during the packaging process.
