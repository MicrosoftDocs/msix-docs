---
description: This article provides all the details you need to manage deploying your MSIX applications in an enterprise and retail environment.  This article is targeted at enterprise and IT Pros.
title: Manage your MSIX deployment Overview
ms.date: 07/12/2026
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

## MSIX Install operations

To plan deployments, especially on multi-user machines, it helps to understand what Windows does under the covers when a package is installed. Installing an MSIX package is really two distinct operations: staging and registration.

### Staging

Staging places the package's files in the correct location on the machine (under `%ProgramFiles%\WindowsApps`). Staging happens once per machine, regardless of how many users will ultimately use the app, and it doesn't require a user to be signed in. Because the payload is shared, staging the same package for a second user doesn't copy the files again.

### Registration

Registration sets up the package for a specific user so that user is ready to run it. It creates the user's app data, wires up file type associations, and adds Start menu entries. Registration is performed once per user and can only happen while that user is signed in.

Because staging and registration are separate, the same staged package can be registered for many users without re-copying its files.

## How MSIX deployment works

The sections below explain how the Staging and Registration operations combine to support provisioning (installing an app for every user) and how updates reach other users on the machine.

### Provisioning

Provisioning is how you make a package install automatically for every user of a device. Provisioning does little more than:

1. Stage the package, then
1. Set a flag indicating the package should be registered for all users, then
1. Raise an event telling the system to register the provisioned package for users who are already signed in.

Windows also registers provisioned packages automatically the next time each user signs in. Note that if a package has already been provisioned for a user and that user uninstalls it, it isn't automatically reinstalled for them. On Windows 10, version 2004 and later, re-provisioning the package reinstalls it; on earlier versions, an administrator must force provision it. For the tools used to provision packages (DISM, provisioning packages, and PowerShell), see [Preinstalling packaged apps](deploy-preinstalled-apps.md).

### How updates reach other users

When a package is updated, it's staged and registered for the user who triggered the update. Every other user who has the app registered also ends up with the newer version, typically the next time the package is registered for them (for example, at their next sign-in). In other words, if a package is updated for one user, you can assume the same version will apply to all other users in the near future. For how the platform minimizes what's downloaded during an update, see [App package updates](../app-package-updates.md).
