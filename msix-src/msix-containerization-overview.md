---
description: An overview of the MSIX containerization model, including app isolation, file and registry virtualization, and the two trust levels available to packaged apps.
title: MSIX containerization overview
ms.date: 04/15/2026
ms.topic: concept-article
keywords: windows 10, windows 11, msix, container, virtualization, isolation, appcontainer, trust level
---

# MSIX containerization overview

MSIX uses a containerization model to isolate apps from the rest of the system. Unlike general-purpose container technologies (such as Docker), MSIX containers are not virtual machines and do not require a separate OS image. Instead, Windows redirects an app's file system and registry activity at runtime, keeping apps self-contained while allowing them to interoperate with the rest of Windows normally.

This article explains the isolation model, the two trust levels available to packaged apps, and the benefits this approach provides.

## What MSIX containerization is (and isn't)

When you install an MSIX package, Windows places the app's files in a protected location (`C:\Program Files\WindowsApps\`) that the app itself cannot modify. At runtime, Windows provides the app with a *virtualized view* of the file system and registry: reads go to the package's install location, and writes are redirected to per-user locations that Windows manages.

This means:

- The app's install files are never modified at runtime.
- App state is stored separately from app binaries — making updates, repairs, and uninstalls clean and reliable.

> [!NOTE]
> MSIX containers are a Windows runtime feature, not a separate operating system environment. The app runs as a native Windows process — it just has a virtualized view of certain system resources.

## Two trust levels

Packaged apps run at one of two trust levels, which determine the degree of isolation:

### Full trust (medium integrity)

Full trust apps run with the same permissions as a standard desktop app. Windows provides *package identity* and some file/registry virtualization, but the app can still access most system resources directly.

This is the default for apps converted from Win32 installers using the MSIX Packaging Tool, and for most WinUI 3 desktop apps.

### AppContainer (partial trust)

AppContainer apps run in a strictly isolated environment. The process and its children can only access resources that are explicitly granted. Windows enforces isolation using file system and registry virtualization, and AppContainer security boundaries.

UWP apps always run in an AppContainer. Desktop apps can also opt into AppContainer by declaring it in the app manifest.

> [!TIP]
> AppContainer apps get the strongest security guarantees, but require more work to configure correctly, especially for apps that rely on broad system access. See [MSIX AppContainer apps](msix-container.md) for configuration steps.

## Benefits of the MSIX containerization model

| Benefit | Description |
|---|---|
| **Clean uninstall** | Because Windows tracks all app state separately, uninstalling an MSIX package removes all app files, registry entries, and system changes — with no leftover artifacts. User-created files are preserved. |
| **Reliable updates** | App binaries are read-only at runtime. Updates replace the package atomically, and the app can be rolled back if needed. |
| **No registry pollution** | Registry writes from the app go to a per-user virtual hive, not to `HKEY_LOCAL_MACHINE`. This prevents the registry sprawl common with traditional Win32 installers. |
| **Security isolation** | AppContainer apps are restricted to explicitly granted resources, reducing the impact of a compromised process. |
| **Integrity enforcement** | Windows can detect tampering with package files at runtime. If a package is tampered with, Windows blocks launch and triggers repair. See [Sign an MSIX package overview](package/signing-package-overview.md). |

## Virtualization scope

Not all file system and registry locations are virtualized. Windows applies different behavior depending on the location being accessed:

- Writes to **package install files** are blocked — the install location is read-only.
- Writes to **system locations** (such as `C:\Windows\`) are blocked for AppContainer apps.
- Writes to **user profile locations** (such as `AppData`) are redirected to per-package locations for AppContainer apps, or pass through for full-trust apps.
- Reads from **packaged VFS locations** are served from the package itself, making the app behave as if it were installed to traditional locations (such as `C:\Program Files\`).

For the full technical detail on how file system and registry virtualization works, see [Understanding how packaged desktop apps run on Windows](desktop/desktop-to-uwp-behind-the-scenes.md).

## Shared containers

By default, each MSIX package runs in its own isolated container. For enterprise scenarios where multiple packages need to share a runtime environment — for example, a main app and a customization package — Windows supports *shared package containers*. A shared container gives a defined set of packages a merged view of each other's virtual file system and registry.

This is an enterprise-only feature requiring administrative privileges. See [Shared package container](manage/shared-package-container.md).

## Flexible virtualization

Starting in Windows 11, apps can selectively opt specific file system folders or registry keys out of virtualization — making those locations visible to other apps and persisting them across uninstall. This is useful for apps that need to share configuration or data with other apps while still benefiting from MSIX packaging.

See [Flexible virtualization](desktop/flexible-virtualization.md).

## Related articles

- [Understanding how packaged desktop apps run on Windows](desktop/desktop-to-uwp-behind-the-scenes.md)
- [MSIX AppContainer apps](msix-container.md)
- [Flexible virtualization](desktop/flexible-virtualization.md)
- [Shared package container](manage/shared-package-container.md)
- [Sign an MSIX package overview](package/signing-package-overview.md)
