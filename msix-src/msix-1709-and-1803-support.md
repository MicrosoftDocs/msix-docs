---
author: c-don
title: MSIX support on builds 1709 and 1803
description: This article summarizes the support for MSIX with our latest updates as of 1/22/2019.
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: windows 10, uwp, msix, 1709, 1803
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---


# MSIX support on Windows 10 builds 1709 and 1803

This article summarizes the MSIX support and limitations with our latest updates.

By popular demand, we have added support for MSIX on more Windows 10 versions. Most notably, this covers versions 1709 and 1803. This support allows users to deploy MSIX packages on earlier Windows versions, while taking advantage of all benefits of MSIX, including containerization and security through certificates. To take advantage of these updates, make sure you have the 1.0.30311 or later version of App Installer and the latest servicing fixes on your 1709 and 1803 devices.

Below, we discuss features and limitations of MSIX support on the earlier OS versions.

##  MSIX double-click support
One of the benefits of deploying an MSIX on version 1809 and later is that the user can install the package by clicking on it. With the latest version of the App Installer - 1.0.30311.0 - the ability to install an MSIX package by clicking on it is available on 1709 and 1803 as well. 

Clicking on the package provides the same installation support as in 1809, namely Microsoft's App Installer application shows UI to the user that guides them through the app installation. The App Installer is pre-installed and gets updates from the store, so we can always bring you the best installation experience. 

Below is a summary of the click-to-install MSIX support available on 1709 and 1803.

### Support matrix

| OS Version|.msix|.msixbundle|.appx|.appxbundle|
|:-------------:|:--------:|:--------:|:--------:|:--------:|
| Win 10 1709(16299) | &#x2713; | &#x2717; | &#x2713; | &#x2713; | 
| Win 10 1803(17134) | &#x2713; | &#x2717; | &#x2713; | &#x2713; |
| Win 10 1809(17763) and above | &#x2713; | &#x2713; | &#x2713; | &#x2713; |


> [!NOTE] 
> MSIXbundles are not supported on Windows 10 versions 1709 and 1803.  Developers looking to build packages compatible with these Windows versions can leverage the .appxbundle technology.

## MDM support
Microsoft Intune and SCCM supports MSIX installation on 1709 and 1803. MSIX can be installed on these builds in the same way as it can in 1809 and later. 

## Store support
The minimum supported OS version of an MSIX is listed in the manifest file of the package as MinVersion in the TargetDeviceFamily element. For example an MSIX package may list MinVersion="10.0.17701.0" as the min supported version, which means that the MSIX can run on this and later versions of the OS.

On 1709, 1803 and 1809 we support the mainstream enterprise deployment scenarios. These include installation through System Center Configuration Manger (SCCM), Microsoft Intune, scripting via PowerShell or double-click installation.

Currently MSIX installation through the Microsoft Store and Microsoft Store for Business require Windows 10 version 1809.

## Packaging & signing
Currently to package and sign an MSIX, you need the 1809 SDK. Packaging and signing can be done via the SDK command line tools, the MSIX Packaging Tool or Visual Studio. 

## Auto elevation
In rare instances, some apps require elevated privileges. 

Users running MSIX on builds later than 1809 can use elevated privileges just by clicking on the app tile. The plaftorm detects that elevation is needed and automatically provides it when the user has the necessary credentials. 

On builds 1709 and 1803, users can still run apps with elevated privileges, however they need to explicity select to run the app as an admin. One way to do that is by right-clicking on the app's tile and selecting the "Run as administrator" option.

## Signature verification
As mentioned earlier, we enforce proper signing of MSIX packages on all Windows versions 1709 and later. However, on the older Windows versions (1709 and 1803), IT Pros need to follow a few extra steps to verify the signature before releasing an app for their end users. Note, for the end user the MSIX experience does not change in any way. The end-user continues to install, launch and use the application as before, since the signature verification is handled by the IT Pros. 

On Windows 10 versions 1809 and later, an IT Pro can verify the app's signature either from PowerShell or through the properties of the MSIX package. 

However, on versions 1709 and 1803 IT Pros cannot verify the signature from the MSIX package's properties, unless the 1809 SDK is installed. If the IT Pro has the 1809 SDK on a device with Windows 1709 through 1803, the signature can be verified through SDK tools from PowerShell. 
