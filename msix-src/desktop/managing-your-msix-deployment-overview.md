---
description: This article provides all the details you need to manage deploying your MSIX applications in an enterprise and retail environment.  This article is targeted at enterprise and IT Pros.
title: Manage your MSIX deployment Overview
ms.date: 08/26/2026
ms.topic: concept-article
keywords: windows 10, deployment, msix
ms.assetid:  
---

# Manage your MSIX deployment

Packaging your application is only half the battle. Next you need to be able to deploy your application to your users. How you deploy your application depends on who your customer is.  This section, Managing your MSIX deployment, will discuss deployment of MSIX packages for both enterprise and retail markets. It will provide links and tips and tricks to ensuring a successful experience. 

In order to successfully deploy MSIX, you need to consider the following:
* Who is my customer?
* What dependencies do I have?
* How will I provide the best support for my customer?

## Who is my customer?
How you deploy often depends on who is your customer and your role as a developer or administrator.   It is important to identify your role to know what tools to use.

### IT Pros
IT Pros typically use a software management system to install and manage their apps.  In the [Distribute your MSIX in an enterprise environment](managing-your-msix-deployment-enterprise.md) section we will discuss:
* Using Microsoft Configuration Manager and Intune to manage your apps
* Using provisioning and DISM to preconfigure machines with MSIX
* Controlling app access with Group Policies and AppLocker
* Distributing apps through the web or Microsoft Intune

### Developers
Developers have different tools to distribute their applications.  In the [Distribute your MSIX in a retail environment](managing-your-msix-deployment-retail.md) section we will discuss:  
* Microsoft Store
* Web Installer
* Using GitHub Actions or Azure Pipelines for CI/CD (App Center retired March 2025)

## Dependencies
MSIX is a robust and reliable modern app install experience. The MSIX experience delivers a 99.96% successful install rate.  But there are some caveats. MSIX is not supported by default on all versions of Windows, and the supported feature set may very depending on which version of Windows 10 you are deploying to.  In the [Plan for your deployment](managing-your-msix-deployment-targetdevices.md) section we will discuss the importance of understanding the target device that the MSIX will be deployed to. 

## Providing support for my customer
Though MSIX has a 99.96% successful install rate, you still need to plan how to support your customer.  In the [MSIX Validation and Troubleshooting section](managing-your-msix-deployment-troubleshooting.md) we discuss tools available for you to diagnose installation issues.

## How MSIX deployment works

On a multi-user device, installing an MSIX package involves two separate operations: staging the package on the device and registering it for a user.

### Staging

Staging stores the package payload in `%ProgramFiles%\WindowsApps`. This operation is device-wide and doesn't require a user account to exist. Windows stages a given package payload once, so registering that package for another user doesn't create another copy of the payload.

### Per-user registration

Registration makes a staged package available to a specific user. Registration creates the user-specific package data and Windows integrations, such as file type associations and Start menu entries. A package must be registered separately for each user who runs its applications.

For a package added interactively, staging and registration can occur as part of the same add operation. For a preinstalled or provisioned package, Windows registers the package for a user when that user signs in.

### Provisioning for all users

[Provisioning](deploy-preinstalled-apps.md) provides the MSIX equivalent of installing for all users. Windows stages a package and adds its package family to the device's provisioned list. Provisioning tracks the package family, not a specific package version.

Provisioning doesn't require every user profile to be loaded at the time of deployment. Windows registers the highest applicable staged version in the provisioned family for each user at sign-in. Therefore, staging a newer version of a provisioned package is sufficient; you don't have to provision that version again.

### User removal and package lifetime

When a user uninstalls an MSIX package, Windows removes that user's registration and user-specific integrations. This action doesn't unregister the package for other users or remove its family from the provisioned list.

The staged payload can remain on the device while another user, provisioning, a dependency, or another package reference still requires it. Windows removes the payload after its remaining references are gone; removal isn't necessarily immediate. If a user removes a regularly provisioned package, a package update doesn't automatically reinstall it for that user. An administrator can use force provisioning when the package must be restored for all users.

To remove a provisioned package from future user registrations, an administrator must deprovision it. Deprovisioning alone doesn't remove registrations that already exist for users.

### How updates reach users

When an update stages a newer package version, the user who initiated the update is registered to that version as part of the update. Other users aren't updated on a fixed timer. At each user's next sign-in, Windows compares the user's registered packages with the staged packages and registers a newer applicable version when one is available.

This behavior lets one staged version serve multiple users while keeping registration user-specific. For information about how Windows minimizes the payload downloaded for an update, see [App package updates](../app-package-updates.md).


