---
Description: This article provides details that need to be considered when deploying your MSIX packages on Windows devices in an enterprise environment.  This article is targeted at enterprise and IT Pros.
title: Plan for your deployment.
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---
  
# Plan for your deployment

No matter if you are targeting the consumer market or enterprise, the key to successful distribution is knowing the devices your deployment is targeting. Depending on the platform you are targetting, you may have additional dependencies that need to be resolved. Some enterprise companies have a single operating system distributed through out the organization. Others have a mixed collection of hardware and operating systems. Inorder to be succcesful in an mixed environment it is important to create a solution that will install easily on all operating systems while limiting the variations in installer technologies. 

All developers also need to know the minimum supported operating system they want to target.  Targeting the lowest common denominator of the operating system, may get you the best potential reach, but often earlier releases of the operating system may not support certain API calls your application is built using.

## MSIX Platform Support
MSIX was introduced to Windows 10 version 1709 (10.0.16299.0) and greater.  This means if you are using the basic MSIX functionality and targeting Windows 10 version 1709 or greater, it will just work.  For a complete list of supporting operating systems and supporting features, see [Supported Platforms.](../supported-platforms.md)

## Services packaged in MSIX
The ability to package services in MSIX were introduced in Windows 10 Client 2004 (10.0.19041.0) and greater. Therefore if your application is using services packaged in MSIX it is limited to deployment on those operating systems. To learn more about using MSIX Package Services in MSIX, see [Convert an installer that includes services.](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services)

## Server support for MSIX Packages
MSIX is not built into Windows Server.  MSIX is however supported on Window 10 Server with Desktop Experience builds 1709 and greater when [AppInstaller application](https://www.microsoft.com/p/app-installer/9nblggh4nns1) is installed.  If you are targeting earlier builds of the server, you must also install MSIX Core.  For information on MSIX Core, see [MSIX Core.](../msix-core/msixcore.md)

## Windows 10 1703 and earlier support for MSIX Packages
If you are targeting earlier versions of Windows, than Windows 10 Client 1709 (10.0.16299.0), you will need to use [MSIX Core.](https://docs.microsoft.com/windows/msix/msix-core/msixcore) By installing MSIX Core on the earlier Windows editions, you will be able to deploy and run MSIX applications. 

For a complete list of supporting operating systems and supporting features, see [Supported Platforms.](../supported-platforms.md) 

## Upgrade, downgrade and architecture considerations
MSIX packages can be upgraded, downgraded, or repaired when the original package is reinstalled.  For efficiency, when downgrading, MSIX does a differential update which means that there is no re-download of the old payload.

When updating an existing package, there some are additional factors that you should consider.  MSIX bundles and MSIX packages can be architecture specific.  While you can upgrade and downgrade apps between architecture, as shown in the table below, you cannot reinstall the same version of different architectures.  

|Installed (version) |  Upgrade or Reinstall version  | Behavior |    Result|
| :---------------: | :-----------------------: | :----------------------:|    :----------------------:|  
| x86 (1.0)   |      x86 (1.0)              | Reinstall | Supported
| x86 (1.0)   |      x86 (3.0)              | Upgrade | Supported
| x86 (1.0)  |      x64 (1.0)              |  Reinstall |<b>Not Supported</b>
| x86 (1.0)  |      x64 (3.0)              |  Upgrade | Supported
| x86 (3.0)   |      x86 (1.0)              | Downgrade | Supported
| x86 (3.0)  |      x64 (1.0)              |  Downgrade | Supported

### Downgrade
When uninstalling or downgrading MSIX, MSIX preserves the user's appdata.  Therefore it is important to note that unless that data created by the newer app is backward compatible, accessing the data with the downgraded app could present an issue.  If the data is not backwards compatible, you may not wish to allow the user to downgrade.

To learn more about how you can control the update settings for your apps, see [Configure update settings in the App Installer file](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services)

### MSIX Bundles
MSIX bundles are packages that are designed to contain multiple architectures.  MSIX packages on the other hand only support a single  architecture.  MSIX bundles can upgrade be used to upgrade or downgrade MSIX packages, but the reverse is not true.  You cannot upgrade or downgrade a MSIX bundle with a MSIX package. 

To learn more about creating bundles, see [Bundle MSIX packages](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages)
