---
description: This article provides all the details you need to manage deploying your MSIX applications in an enterprise and retail environment.  This article is targeted at enterprise and IT Pros.
title: Manage your MSIX deployment Overview
ms.date: 08/27/2026
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

## Choose a deployment mechanism for the execution context

MSIX package registration is per user. Registration creates user-specific package state and makes the package's applications available in that user's session. A package can also be staged on the device without being registered, and a package family can be provisioned so that Windows registers the latest staged package for users.

The LocalSystem account, also shown as **SYSTEM**, can't have packages registered. A process that runs as LocalSystem therefore can't use a per-user deployment mechanism to install an MSIX package for the signed-in user. This limitation commonly affects services and device-context management agents.

| Goal | Execution context and supported mechanism |
| --- | --- |
| Install for the current user | Run the [App Installer app](../app-installer/app-installer-root.md), [Add-AppxPackage](/powershell/module/appx/add-appxpackage), or [`PackageManager.AddPackageAsync`](/uwp/api/windows.management.deployment.packagemanager.addpackageasync) in that user's interactive context. These mechanisms stage and register the package for the caller. Don't run them as LocalSystem with the expectation that they register the package for another user. Check the selected API overload's requirements before calling it. Packaged callers need the applicable restricted package-management capability; an unpackaged full-trust caller is still subject to deployment permissions and registers the package for its own user context. |
| Make the package available to users on a device | From an elevated administrator or LocalSystem context, use [Add-AppxProvisionedPackage](/powershell/module/dism/add-appxprovisionedpackage) or the corresponding [DISM app package servicing command](/windows-hardware/manufacture/desktop/dism-app-package--appx-or-appxbundle--servicing-command-line-options). Provisioning stages the package and adds its package family to the provisioned list. Windows registers an applicable staged package when registration is initialized for a user. Provisioning doesn't register a package for LocalSystem. A user who previously removed the package might not receive it again through regular reprovisioning; for that scenario, see [Force Provisioning](deploy-preinstalled-apps.md#force-provisioning). |
| Stage package files only | Use a deployment API stage operation, such as [StagePackageAsync](/uwp/api/windows.management.deployment.packagemanager.stagepackageasync). Staging is machine-wide, but staging alone doesn't make the package's applications available to a user; registration or provisioning is still required. |

If deployment must remain per user, configure the management tool to run in the target user's context. Otherwise, use provisioning to make the package available for per-user registration; provisioning isn't machine-wide package registration. For more information about the relationship between staging, registration, and provisioning, see [Preinstalling packaged apps](deploy-preinstalled-apps.md) and [MSIX per-user versus all-users deployment](https://devblogs.microsoft.com/insidemsix/msix-per-user-vs-all-users/).

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


 
