---
description: Guidance for replacing an existing installer-based desktop app (.exe, .msi, or ClickOnce) with its MSIX version, including how the two installs coexist, how to remove the previous version, and which deployment technology to use.
title: Replace an existing installer-based app with MSIX
ms.date: 07/03/2026
ms.topic: concept-article
keywords: windows 10, deployment, msix, migrate, replace, installer, msi, clickonce
ms.assetid:  
---

# Replace an existing installer-based app with MSIX

When you move an existing desktop application to MSIX, you often have a version already installed on your users' machines through a classic installer, such as an **.exe** setup program, a Windows Installer (**.msi**) package, or a **ClickOnce** deployment. This article explains how the MSIX version relates to that existing install, how to remove the previous version, and how to choose a deployment technology for the rollout.

Before you plan the transition, package your application. For help preparing your installer for conversion, see [Know your installer](../packaging-tool/know-your-installer.md) and [Create an MSIX package](../packaging-tool/create-an-msix-overview.md).

## The MSIX version installs side by side

An MSIX package has a unique [package identity](../overview.md) that the operating system tracks. A classic **.exe**, **.msi**, or ClickOnce installation does not have this identity, so Windows treats the MSIX version as a separate app.

Because of this, keep the following in mind:

* Installing the MSIX package does **not** upgrade or remove the existing unpackaged installation. There is no in-place upgrade from a classic installer to MSIX &mdash; it is a *replace*, not an upgrade.
* Both versions can be present on the machine at the same time, which can result in duplicate Start menu entries and file-type associations.
* MSIX version comparison and upgrade logic (see [Manage your MSIX deployment](managing-your-msix-deployment-overview.md)) applies only between MSIX packages, not between an MSIX package and a classic installation.

Plan to remove the previous version as part of your rollout so that users end up with only the MSIX version.

## Remove the previous version

The recommended approach is to have the MSIX version drive the removal of the older installer-based app, so that a single deployment transitions the user. There are two common patterns.

### Uninstall from the new app

Have your MSIX-packaged app detect the previous installation on first run and uninstall it. For example, the app can look up the legacy app's uninstall string in the registry (or the Windows Installer product code for an **.msi**) and invoke it silently.

Keep these considerations in mind:

* An MSIX-packaged app runs with the current user's privileges. Uninstalling a **per-user** installation works without a prompt, but silently removing a **per-machine** (all-users) **.msi** or **.exe** installation requires administrator elevation, which a packaged app can't obtain silently.
* Only remove software that your app owns. Verify the legacy app's identity (for example, its product code or publisher) before uninstalling it.

This pattern works well for consumer and retail distribution, where an IT management tool isn't available. See [Distribute your MSIX in a retail environment](managing-your-msix-deployment-retail.md).

### Uninstall with your management tool

In a managed environment, use your device or application management tool to remove the legacy **.exe**, **.msi**, or ClickOnce app as part of the MSIX rollout. For example, sequence an uninstall of the old application before (or alongside) the MSIX deployment in Microsoft Intune or Microsoft Configuration Manager.

For details, see [Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md), [Microsoft Intune](managing-your-msix-deployment-intune.md), and [Microsoft Configuration Manager](managing-your-msix-deployment-configmgr.md).

## Choose a deployment technology

How you deliver the MSIX version depends on your audience. The following options are covered in [Manage your MSIX deployment](managing-your-msix-deployment-overview.md):

| Deployment technology | Best for | Learn more |
|---|---|---|
| Microsoft Store | Consumer apps and broad distribution | [Distribute your MSIX in a retail environment](managing-your-msix-deployment-retail.md) |
| App Installer (web install) | Distributing and auto-updating apps from your own website | [Install MSIX with App Installer](../app-installer/app-installer-root.md) |
| Microsoft Intune | Cloud-managed enterprise devices | [Microsoft Intune](managing-your-msix-deployment-intune.md) |
| Microsoft Configuration Manager | On-premises managed enterprise devices | [Microsoft Configuration Manager](managing-your-msix-deployment-configmgr.md) |
| MSIX Core | Bringing MSIX (including from a ClickOnce setup.exe) to earlier versions of Windows | [MSIX Core](../msix-core/msixcore.md) |

If you're moving from ClickOnce specifically, you can also reuse a ClickOnce-style setup.exe experience to bootstrap the MSIX install on down-level Windows. See [Create an MSIX package with MSIX Core from source code](../msix-core/msixcore-clickonce-solution.md).

## Related content

* [Know your installer](../packaging-tool/know-your-installer.md)
* [Manage your MSIX deployment](managing-your-msix-deployment-overview.md)
* [Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md)
* [Distribute your MSIX in a retail environment](managing-your-msix-deployment-retail.md)
