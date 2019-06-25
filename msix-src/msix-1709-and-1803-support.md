---
author: c-don
title: MSIX support on Windows 10 version 1709 and 1803
description: This article summarizes the support for MSIX with our latest updates as of 1/22/2019.
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX support on Windows 10 version 1709 and 1803

This article summarizes the MSIX support and limitations with our latest updates.

By popular demand, we have added support for MSIX on more Windows 10 versions. Most notably, this covers versions 1709 and 1803. This support allows users to deploy MSIX packages on earlier Windows versions, while taking advantage of all benefits of MSIX, including containerization and security through certificates. To take advantage of these updates, make sure you have the 1.0.30311 or later version of App Installer and be on Windows 10 1709 update or later. 

Below, we discuss features and limitations of MSIX support on the earlier OS versions.

##  MSIX double-click support

One of the benefits of deploying an MSIX package on Windows 10 version 1809 and later is that the user can install the package by clicking on it. With the latest version of the App Installer (1.0.30311.0) the ability to install an MSIX package by clicking on it is available on Windows 10 version 1709 and 1803 as well.

Clicking on the package provides the same installation support as in 1809, namely App Installer guides the user through the app installation. App Installer is pre-installed and gets updates from the Microsoft Store, so we can always bring you the best installation experience.

App Installer can be downloaded for offline use in the enterprise from Microsoft Store for Business [web portal](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1). You can learn more about offline distribution [here](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

Below is a summary of the click-to-install MSIX support available on Windows 10 version 1709 and 1803.

### Support matrix

| OS Version|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10, version 1709 (build 16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10, version 1803 (build 17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10, version 1809 (build 17763) and later | &#x2713; | &#x2713; | &#x2713; | &#x2713; |

> [!NOTE]
> The .msixbundle format is not supported on Windows 10 version 1709 and 1803.  Developers looking to build packages compatible with these Windows versions can leverage the .appxbundle format.

## MDM support

Microsoft Intune and System Center Configuration Manger (SCCM) supports MSIX installation on Windows 10 version 1709 and 1803. MSIX packages can be installed on these versions in the same way as it can in Windows 10 version 1809 and later.

## Store support

The minimum supported OS version of an MSIX package is listed in the manifest file of the package as `MinVersion` in the `TargetDeviceFamily` element. For example an MSIX package may list `MinVersion="10.0.17701.0"` as the minimum supported version, which means that the MSIX package can run on this and later versions of the OS.

On Windows 10 versions 1709, 1803, and 1809, we support the mainstream enterprise deployment scenarios. These include installation through SCCM, Microsoft Intune, PowerShell or double-click installation.

Currently, MSIX installation through the Microsoft Store and Microsoft Store for Business require Windows 10 version 1809.

## Packaging and signing

Currently, to package and sign an MSIX file, you need the Windows 10 version 1809 SDK. Packaging and signing can be done via the Windows 10 SDK command line tools (MakeAppx.exe and SignTool.exe), the MSIX Packaging Tool, or Visual Studio.

## Auto elevation

In rare instances, some apps require elevated privileges.

Users running an MSIX package on Windows 10 version 1809 and later can use elevated privileges just by clicking on the app tile. The platform detects that elevation is needed and automatically provides it when the user has the necessary credentials.

On Windows 10 versions 1709 and 1803, users can still run apps with elevated privileges. However, they need to explicitly select to run the app as an administrator. One way to do that is by right-clicking on the app's tile and selecting the "Run as administrator" option.

## Digital certificate verification

As mentioned earlier, we enforce proper signing of MSIX packages on Windows 10 version 1709 and later. However, on Windows 10 versions 1709 and 1803, IT pros need to follow a few extra steps to verify the digital certificates before releasing an app for their end users. For the end user, the MSIX experience does not change in any way. The end-user continues to install, launch and use the app as before, since the certificate verification is handled by the IT pro.

On Windows 10 version 1809 and later, an IT pro can verify the digital certificate of an MSIX package either from PowerShell or through the properties of the MSIX package.

However, on Windows 10 versions 1709 and 1803, IT pros cannot verify the certificate from the MSIX package's properties unless the Windows 10 version 1809 SDK is installed. If the IT pro has the  Windows 10 version 1809 SDK on a device with Windows 10 version 1709 through 1803, the signature can be certificate can be verified through SDK tools from PowerShell.
