---
title: Package registration and lifetime
description: Understand how registration, provisioning, dependencies, and active processes determine when Windows can remove staged MSIX package files.
ms.date: 09/30/2026
ms.topic: concept-article
keywords: msix, package lifetime, staged package, registration, deregistration, destage, remove package
---

# Package registration and lifetime

Removing an MSIX package for a user doesn't always immediately delete its staged files. Windows manages staged packages by tracking whether users, other packages, or running processes still reference them.

Use the following terms when describing package lifetime:

- **Stage**: Place a package's payload in a Windows-managed installation store.
- **Register**: Make a staged package available to a user and create its user-specific integrations.
- **Remove or deregister**: Remove a user's registration and integrations.
- **Destage**: Delete the staged package payload from storage.

A removal operation can deregister a package without immediately destaging it.

## Strong references

Windows can't destage a package while a strong reference remains. Common strong references include:

- A user's explicit package registration.
- Provisioning that makes the package available to users.
- A static dependency declared through `PackageDependency`.
- A host-runtime dependency.
- A persisted dynamic dependency that only the package can satisfy.
- A process that is currently using package content.
- A system pin.
- Unified Update Platform (UUP) product membership.

Framework packages commonly remain staged because other packages depend on them. Multiple Framework-package versions can remain side by side while applications transition between versions.

## Weak references

A weak reference doesn't independently keep a package staged. Resource-package relationships are the primary example. If only weak references remain, Windows can remove the package and its weak relationships.

## What happens during removal

When you remove a package, Windows first removes the applicable user registration and user-specific integrations. It then evaluates whether strong references remain:

1. If a strong reference remains, the package stays staged.
1. If no strong references remain, the package becomes eligible for destaging.
1. Windows removes the payload when deployment next evaluates and collects the unreferenced package.

Destaging can occur after the removal operation completes. Some reference changes happen outside a deployment operation, so payload cleanup isn't always immediate.

If a process is using the package, removal normally requests application shutdown. To avoid interrupting the process, call [PackageManager.RemovePackageAsync](/uwp/api/windows.management.deployment.packagemanager.removepackageasync) with [RemovalOptions.DeferRemovalWhenPackagesAreInUse](/uwp/api/windows.management.deployment.removaloptions). Windows completes the removal after the package is no longer in use.

## PowerShell behavior

`Remove-AppxPackage` removes a registration. Don't use the absence of a user registration as proof that Windows has already deleted every staged file.

Use `Get-AppxPackage -AllUsers` and provisioning commands when diagnosing references held by other users or by device provisioning. Removing a provisioned package affects future registrations but doesn't remove registrations that already exist for users.

## Related content

- [Package installation blocked because the app is in use](../msix-troubleshooting-guide.md#package-installation-blocked-because-the-app-is-in-use)
- [Managing MSIX with PowerShell](../desktop/powershell-msix-cmdlets.md)
- [Remove and Package Lifetime](https://devblogs.microsoft.com/insidemsix/msix-package-lifetime/)
- [Remove Is Not Uninstall](https://devblogs.microsoft.com/insidemsix/remove-is-not-uninstall/)
