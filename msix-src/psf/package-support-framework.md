---
description: Get started with the Package Support Framework. Work through the four-step workflow that fixes a desktop application that fails in an MSIX container.
title: Fix issues that prevent your desktop application from running in an MSIX container
ms.date: 08/24/2026
ms.topic: get-started
keywords: windows 10, windows 11, msix, psf, package support framework, runtime fix, fixup
---

# Get started with the Package Support Framework

The [Package Support Framework (PSF)](package-support-framework-overview.md) is an open-source kit
that applies fixes to an existing desktop application, without modifying its code, so that the
application can run in an MSIX container. The PSF helps an application follow the practices of the
modern runtime environment.

The PSF applies to a full-trust desktop application in an MSIX package. It doesn't apply to a UWP
application.

This article is the entry point for the four-step workflow. Each step is a separate article, so you
can work through the whole workflow or jump to the step you need.

## Before you begin

Gather the following before you start:

| What you need | Why |
|---|---|
| Your desktop application packaged as an MSIX package, signed and installed on a test device | The PSF fixes runtime behavior in the container, so you need a package that reproduces the failure. See [Create an MSIX package](../packaging-tool/create-an-msix-overview.md). |
| [Process Monitor](/sysinternals/downloads/procmon) | Identifies the failing file system and registry calls. |
| The PSF binaries from the [Microsoft.PackageSupportFramework](https://www.nuget.org/packages/Microsoft.PackageSupportFramework) NuGet package | Provides the launcher, the runtime manager, and the runtime fixes. For what changed in each release, see [Package Support Framework releases](package-support-framework-overview.md#package-support-framework-releases). |
| The [Windows SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) | Provides MakeAppx and SignTool for repackaging and signing. |
| Visual Studio, [DebugView](/sysinternals/downloads/debugview), and [Process Explorer](/sysinternals/downloads/process-explorer) | Optional. Needed to debug, extend, or write a runtime fix, and to verify injection. |

For the components of the PSF, the fixes it includes, and the current release, see the
[Package Support Framework overview](package-support-framework-overview.md).

## The Package Support Framework workflow

| Step | What you do | Outcome |
|---|---|---|
| [Step 1: Identify compatibility issues](psf-identify-issues.md) | Reproduce the failure and capture it with Process Monitor or the Trace Fixup. | The failing call, the requested path, and the result, such as **ACCESS DENIED**. |
| [Step 2: Find a runtime fix](psf-find-a-runtime-fix.md) | Match the failure to a fix that ships with the PSF, a community fix, or a new fix you write. | The fixup DLL and the configuration it needs. |
| [Step 3: Apply a runtime fix](psf-apply-a-runtime-fix.md) | Add the PSF binaries to the package, point the manifest at the PSF launcher, author *config.json*, and repackage. | A signed MSIX package that applies the fix, and a verified result. |
| [Step 4: Debug or extend a runtime fix](psf-debug-a-runtime-fix.md) | Set up a Visual Studio solution to debug the fix, extend it, or build a new one. | A runtime fix you can step through and change. |

Steps 1 through 3 resolve most issues. Use Step 4 when the applied fix doesn't fully resolve the
failure or when no existing fix matches.

> [!div class="nextstepaction"]
> [Step 1: Identify compatibility issues](psf-identify-issues.md)

## Fixes for common failures

If you already know what the application does wrong, start with the focused walkthrough for that
scenario:

- [Package Support Framework - Working Directory fixup](psf-current-working-directory.md): the
  application can't find files that ship in the package, because the process working directory
  defaults to the `System32` or `SysWOW64` directory.
- [How to fix Package Support Framework Filesystem Write Permission errors](psf-filesystem-writepermission.md):
  the application writes to its install directory under `%ProgramFiles%\WindowsApps` and fails with
  an access denied error.
- [Package Support Framework - Launching Windows apps with parameters](psf-launch-apps-with-parameters.md):
  the application must be started with command-line arguments that a shortcut used to supply.

## Related tasks

- [Automated PSF config generation](psf-integration-with-mpt.md): let the MSIX Packaging Tool detect
  issues and generate the PSF configuration.
- [Apply the Package Support Framework in Visual Studio](package-support-framework-vs.md): add the
  PSF to a package you build from a Visual Studio solution.
- [Run scripts with the Package Support Framework](run-scripts-with-package-support-framework.md):
  run a PowerShell script before or after the application runs.
- [Create a Package Support Framework fixup](create-package-support-framework.md): write replacement
  functions when no existing fix applies.

## Support

Have questions? Ask on the
[Package Support Framework](https://techcommunity.microsoft.com/t5/Package-Support-Framework/bd-p/Package-Support)
conversation space on the MSIX Tech Community site, or open an issue in the
[Package Support Framework repository](https://github.com/microsoft/MSIX-PackageSupportFramework/issues).
