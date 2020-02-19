---
Description: This article describes different ways to install an MSIX app for all users.
title: Install your app for all users
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Install your app for all users with DISM

There are a number of tools that can be used to install an MSIX app for all users. [Deployment Image Servicing and Management (DISM.exe)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism---deployment-image-servicing-and-management-technical-reference-for-windows) is a command-line tool that can be used to service and prepare Windows images, including those used for Windows PE, Windows Recovery Environment (Windows RE) and Windows Setup. DISM can be used to service a Windows image (.wim) or a virtual hard disk (.vhd or .vhdx).

Windows provisioning makes it easy for IT Pros to configure end-user devices without imaging. Using Windows provisioning, IT Pros can easily specify desired configuration and settings required to enroll the devices into management and then apply that configuration to target devices in a matter of minutes. For example, installing an app for all users on a device. 

Starting in Windows version 1809, IT Pros can pre-install through provisioning.   Provisioned apps, will be installed to a central location, %ProgramFiles%\WindowsApps and will immediately be available to registered users. Users that do not have the MSIX registered to their account will not have access.  
For more details on provisioning packages see [Provisioning packages for Windows 10](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages).

## User preferences when provisioning

Provisioning respects the users settings and therefore when a user uninstalls a provisioned app, the only way the provisioned app comes back is if the user reinstalls it. In Windows 10 version 2004, a provisioned app will automatically override the users setting and reinstall the app when re-provisioned.

You should also be careful to prevent the users from installing and updating different versions of the provisioned MSIX package. If the user upgrades or downgrades a provisioned app, by installing the same package from the store, you may get unexpected results.
For more details on MSIX provisioning, see [Create Windows applications in Configuration Manager](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications).

## Removing apps for all users

In addition to installing apps for all users, you can also uninstall an app for all users. The way to do this is through the Package Manager API, [PackageManager.RemovePackageAsync Method](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.removepackageasync).

Alternatively, you can use **AppxRemovePackageForAllUsers(PCWSTR packageFullName, BOOL* reserved)()** method that can be found in the **AppxDeploymentClient.dll** that was introduced in Windows 10 version 1703. However, we recommend using the **PackageManager.RemovePackageAsync** instead.
