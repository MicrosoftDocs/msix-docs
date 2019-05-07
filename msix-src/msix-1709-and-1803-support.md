---
author: c-don
title: MSIX support on 1709 and 1803
description: This article summarizes the support for MSIX with our latest updates as of 1/22/2019.
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---


# MSIX support on Windows 10, 1709 and 1803

This article summarizes the MSIX support and limitations with our latest updates.

By popular demand, we have added support for MSIX on more Windows 10 versions. Most notably, this covers versions 1709 and 1803. This support allows users to deploy MSIX packages on earlier Windows versions, while taking advantage of all benefits of MSIX, including containerization and security through certificates. To take advantage of these updates, make sure you have the 1.0.30311 or later version of App Installer and the latest servicing fixes on your 1709 and 1803 devices.

##  MSIX double-click support
One of the benefits of deploying an MSIX package on version 1809 and later is that the user can install the package by clicking on it. With the latest version of App Installer (1.0.30311.0), the ability to install an MSIX package by clicking on it is available on 1709 and 1803 as well. 

Clicking on the package provides the same installation support as in 1809, namely App Installer guides the user through the app installation. App Installer is pre-installed and gets updated from Microsoft Store, so we can always bring you the best installation experience. 

Below is a summary of the click-to-install MSIX support available on 1709 and 1803.

### Support matrix

| OS Version|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Windows 10, 1709<br/>(build 16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Windows 10, 1803<br/>(build 17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Windows 10, 1809<br/>(build 17763) and later | &#x2713; | &#x2713; | &#x2713; | &#x2713; |


> [!NOTE] 
> The `.MSIXbundle` format is not supported on Windows 10 versions 1709 and 1803. Developers looking to build packages compatible with these Windows versions can leverage the `.appxbundle` technology.

## MDM support
Microsoft Intune and System Center Configuration Manger (SCCM) supports MSIX installation on 1709 and 1803. MSIX packages can be installed on these versions in the same way as it can in 1809 and later. 

## Store support
The minimum supported OS version of an MSIX package is listed in the manifest file of the package as `MinVersion` in the `TargetDeviceFamily` element. For example an MSIX package may list `MinVersion="10.0.17701.0"` as the min supported version, which means that the MSIX ackage can run on this and later versions of the OS.

On 1709, 1803 and 1809, we support the mainstream enterprise deployment scenarios. These include installation through SCCM, Microsoft Intune, PowerShell or double-click installation.

Currently, MSIX installation through Microsoft Store and Microsoft Store for Business require Windows 10 version 1809.

## Packaging & signing
Currently, to package and sign an MSIX file, you need the 1809 SDK. Packaging and signing can be done via the SDK command line tools, MSIX Packaging Tool or Visual Studio. 

## Auto-elevation
In rare instances, some apps require elevated privileges. 

Users running an MSIX package on 1809 and later can use elevated privileges just by clicking on the app tile. The plaftorm detects that elevation is needed and automatically provides it when the user has the necessary credentials. 

On 1709 and 1803, users can still run apps with elevated privileges; however, they need to explicity select to run the app as an administrator. One way to do that is by right-clicking on the app's tile and selecting the "Run as administrator" option.

## Digital certificate verification
As mentioned earlier, we enforce proper signing of MSIX packages on Windows 10 version 1709 and later. However, on the older Windows 10 versions (1709 and 1803), IT pros need to follow a few extra steps to verify the digital certificate before releasing an app for their end users. For the end user, the MSIX experience does not change in any way. The end-user continues to install, launch and use the app as before, since the certificate verification is handled by the IT pro.

On Windows 10 version 1809 and later, an IT pro can verify the digital certificate of an MSIX package either from PowerShell or the Properties dialog box of File Explorer.

However, on versions 1709 and 1803, IT pros cannot verify the certificate from File Explorer, unless the 1809 SDK is installed. If the IT Pro has the 1809 SDK on a device with Windows 1709 through 1803, the certificate can be verified through SDK tools from PowerShell.
