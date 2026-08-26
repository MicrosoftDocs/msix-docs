---
description: Step 2 of the Package Support Framework workflow. Match the failure you captured to a runtime fix that ships with the PSF, a community fix, or a new fix.
title: "Step 2: Find a runtime fix"
ms.date: 08/24/2026
ms.topic: how-to
keywords: windows 10, windows 11, msix, psf, package support framework, fixup, runtime fix, file redirection
---

# Step 2: Find a runtime fix

This is the second step in the [Package Support Framework (PSF) workflow](package-support-framework.md).
After you identify the failing call in [Step 1](psf-identify-issues.md), match that failure to a
runtime fix.

A runtime fix is a DLL that the PSF runtime manager loads into the application process. The DLL
intercepts the function calls that fail in an MSIX container and replaces them with an
implementation that succeeds. You configure each fix in the package's *config.json* file, so a fix
applies to the processes it's configured for, not to the whole package or to the machine. The PSF
runtime is also injected into the child processes that your application starts, and the `processes`
array in *config.json* decides which fixes apply to each of those processes.

## Runtime fixes included with the Package Support Framework

| Runtime fix | Use it when |
|---|---|
| [FileRedirectionFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/FileRedirectionFixup) | The application reads or writes files in a location that isn't writable from the container, such as its own install directory under `%ProgramFiles%\WindowsApps`. The fix redirects the matching paths to a writable per-user location. |
| [RegLegacyFixups](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/RegLegacyFixups) | The application opens registry keys with access rights it doesn't need and fails with an access error, deletes registry keys or values, or expects a registry value that lives outside the package. Remediation types include `ModifyKeyAccess`, `FakeDelete`, `DeletionMarker`, and `Redirect`. |
| [EnvVarFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/EnvVarFixup) | The application depends on an environment variable that a traditional installer would have set on the system or the user. The fix returns the value from *config.json* or from the package registry hive. |
| [DynamicLibraryFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/DynamicLibraryFixup) | The application fails to load a DLL that ships in the package because it loads the DLL by name from a location the loader doesn't search. |
| [ElectronFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/ElectronFixup) | A basic application built on the Electron framework doesn't launch in an AppContainer, that is, in a package that doesn't run with full trust. This fix requires the `CreateFileFromAppW` API, so the device must run Windows 10, version 1803 (10.0.17134) or later. The fix isn't part of the Package Support Framework solution or the NuGet package, so build it from the repository. |
| [TraceFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/tests/fixups/TraceFixup) | You need diagnostics rather than a fix. It reports which functions the application calls and whether the calls succeed. The binaries are in the NuGet package as *TraceFixup32.dll* and *TraceFixup64.dll*. See [Step 1](psf-identify-issues.md#use-the-trace-fixup). |

Some behavior is configured on the PSF launcher rather than on a fixup DLL. Setting the working
directory, passing arguments to the application executable, running a script before or after the
application, and running child processes that aren't part of the package in the package context, with
the `inPackageContext` setting, are all
[PSF launcher](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/main/PsfLauncher/Readme.md)
settings in *config.json*.

The list of fixes changes between releases. For the current set and the configuration schema of
each fix, see the [Package Support Framework repository](https://github.com/microsoft/MSIX-PackageSupportFramework)
and the release notes in [Package Support Framework releases](package-support-framework-overview.md#package-support-framework-releases).

## Match the failure to a fix

Use the evidence you recorded in Step 1.

| What you saw in Step 1 | Fix to start with | Focused walkthrough |
|---|---|---|
| **NAME NOT FOUND** or **PATH NOT FOUND** for a file that ships in the package, requested from `System32` or `SysWOW64` | Set `workingDirectory` on the application in *config.json* | [Package Support Framework - Working Directory fixup](psf-current-working-directory.md) |
| **ACCESS DENIED** with **Desired Access: Generic Write** under `%ProgramFiles%\WindowsApps` | FileRedirectionFixup | [How to fix Package Support Framework Filesystem Write Permission errors](psf-filesystem-writepermission.md) |
| The application requires command-line arguments that a shortcut used to supply | `arguments` on the application in *config.json* | [Package Support Framework - Launching Windows apps with parameters](psf-launch-apps-with-parameters.md) |
| The application starts a helper process whose executable isn't in the package, and that process fails or writes to the wrong location | `inPackageContext` on the application in *config.json* | [PSF launcher readme](https://github.com/microsoft/MSIX-PackageSupportFramework/blob/main/PsfLauncher/Readme.md) |
| **ACCESS DENIED** opening a registry key, or a failed registry delete | RegLegacyFixups | [RegLegacyFixups readme](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/RegLegacyFixups) |
| The application reads an environment variable that no longer exists | EnvVarFixup | [EnvVarFixup readme](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/EnvVarFixup) |
| A DLL that ships in the package fails to load | DynamicLibraryFixup | [DynamicLibraryFixup source](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/DynamicLibraryFixup) |

These pairings are a starting point, not an exhaustive mapping. An application can hit more than one
issue, as the [PSFSample](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/samples/PSFSample)
application does, and you can list more than one fix in the `fixups` array of a single process.

### Example: File Redirection Fixup

The [FileRedirectionFixup](https://github.com/microsoft/MSIX-PackageSupportFramework/tree/main/fixups/FileRedirectionFixup)
redirects attempts to read or write data in a directory that the application can't write to from an
MSIX container. For example, if the application writes a log file to the directory that holds its
own executable, the fix creates and uses that log file in a writable location instead. Configure
which paths are redirected with the `redirectedPaths` element, which accepts three kinds of base
path: `packageRelative` for paths under the package installation folder, `packageDriveRelative` for
paths on the drive where the package is installed, and `knownFolders` for locations that
[SHGetKnownFolderPath](/windows/win32/api/shlobj_core/nf-shlobj_core-shgetknownfolderpath) resolves.

The redirected copy is written per user. Unless you set `redirectTargetBase`, the fix redirects to a
VFS folder under the user's local app data folder, and the first access copies the file there. Each
user therefore works with a separate copy of the data, and that copy lives outside the package.

> [!TIP]
> If your application fails because it writes to its install directory, follow the end-to-end
> walkthrough in [How to fix Package Support Framework Filesystem Write Permission errors](psf-filesystem-writepermission.md).
> It covers capture, configuration, repackaging, and validation for that scenario.

## Runtime fixes from the community

Review the community contributions in the
[Package Support Framework repository](https://github.com/microsoft/MSIX-PackageSupportFramework).
Another developer might have resolved an issue similar to yours and shared a runtime fix.

## When no existing fix applies

If none of the available fixes address your issue, you can write one. A new fix declares replacement
functions for the calls that fail and, optionally, reads its own configuration data from
*config.json*. For details, see
[Create a Package Support Framework fixup](create-package-support-framework.md), then debug it by
following [Step 4](psf-debug-a-runtime-fix.md).

## Next step

> [!div class="nextstepaction"]
> [Step 3: Apply a runtime fix to your MSIX package](psf-apply-a-runtime-fix.md)

## Related content

- [Package Support Framework overview](package-support-framework-overview.md)
- [Create a Package Support Framework fixup](create-package-support-framework.md)
- [Run scripts with the Package Support Framework](run-scripts-with-package-support-framework.md)
