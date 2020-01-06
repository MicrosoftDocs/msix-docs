---
title: Deploy an MSIX package with MSIX Core
description: Describes how to deploy an MSIX package with MSIX Core by using the msixmgr.exe tool.
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Deploy an MSIX package with MSIX Core

[MSIX Core](msixcore.md) brings MSIX deployment to select previous versions of Windows. To get started, first make sure that MSIX Core is installed on target device.

## MSI installation

We recommend using our provided MSI installers to install MSIX Core because they automatically add msixmgr.exe to your search path and associate the MSIX extension with the installer.

You can download the following architecture-specific MSI installers from the **Assets** section on our [release page](https://github.com/microsoft/msix-packaging/releases):

* **msixmgrSetup-x64.msi**
* **msixmgrSetup-86.msi**

> [!NOTE]
> Make sure you choose the correct installer for your device's architecture. This will impact where the installer will store important files. The name of the file may change based on the version of the installer.

## Installing your certificate

MSIX packages are required to be signed. Before installing any MSIX packages, make sure you have installed the certificate you used to sign your packages. You can do this using you normal workflows for installing certificate from your management tool.

If you want to manually install a certificate you can run this command from an elevated command prompt:

```PowerShell
certutil -addstore root <insert certificate.cert>
```

> [!NOTE]
> You should add your trusted certificate under Trusted Root Certification Authority in all scenarios.

## Using the Command Line

Once the tool msixmgr.exe is installed, it can be used to manage your MSIX packages on this machine by searching, installing, and removing. The command line utility msixmgr.exe is intended for system administrators. It is most useful when run from administrator prompt. Not all commands when run from a regular command prompt will display to the console. See below for more details.

### Install

Using command prompt or PowerShell, navigate to the directory that contains msixmgr.exe and run the following command to install your MSIX package. The `-quietUX` parameter can also be added at the end of the command so that users don't see the installer UI.

```PowerShell
msixmgr.exe -AddPackage C:\NotePadPlus\notepadplus.msix -quietUX
```

> [!NOTE]
> This and the following examples use **notepadplus.msix**. This is one of our [sample packages](https://github.com/microsoft/msix-packaging/tree/master/MsixCore/Tests).

### Querying for a specific MSIX Package

Searching for a specific package is possible by **packageFullName**, **packageFamilyName** and/or using wildcards as well. Supported wildcards are *(match any character) and ?(match single character).

```PowerShell
msixmgr.exe -FindPackage notepadplus_0.0.0.1_???__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *epadplus_8wekyb3d8bbw?
```

### Uninstall

To uninstall use the following command:

```PowerShell
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe -quietUX
```