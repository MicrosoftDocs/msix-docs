---
title: Get started with MSIX Core
description: This article provides a step by step instruction on how to leverage the MSIX Core bootstrapper, which creates an application using ClickOnce that will allow your users to just download a setup.exe and install their MSIX app through the MSIX Core Installer.
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Get started with MSIX Core
There are multiple ways to get started with MSIX Core. The first thing to do is to ensure that your Windows device supports MSIX as an installer.

To do this, go to our [release page](https://github.com/microsoft/msix-packaging/releases) and under **Assets** you will find the following: 
1. msixmgr.zip
2. msixmgrSetup-x64.msi
3. msixmgrSetup-86.msi
5. Source code (zip)
6. Source code (tar.gz) 

Download either **msixmgrSetup-x64.msi** or **msixmgrSetup-x86.msi** (depending on your architecture) and install it on your Windows Device. 

## Using the Command Line
The executables can also be manually deployed to a Windows 7 SP1 or higher machine without using the MSI setup project. Download **msixmgr.zip** under **Assets**] on our [release page](https://github.com/microsoft/msix-packaging/releases) and you can find **msix.dll** and **msixmgr.exe**. Place the **msix.dll** and **msixmgr.exe** in the same location as if you were to install MSIX Core with our .msi. 

We've provided [sample packages](https://github.com/microsoft/msix-packaging/tree/master/preview/MsixCore/Tests) in our GitHub for testing purposes. These can be copied over to a Windows 7 SP1 or higher machine and installed, although they will require adding the APPX_TEST_ROOT.cer (in the same tests folder) to the trusted store in order to install. Some of these packages are large and are stored using git lfs (Large File Storage).

```
certutil -addstore root APPX_TEST_ROOT.cer
```

> [!NOTE]
> Replace APPX_TEST_ROOT.cer with your own certificate when you are deploying your own MSIX packages. 

### Install
Using command prompt or PowerShell, navigate to the directory that contains the executables and run the following command to install notepadplus.msix. The -quietUX parameter can also be added at the end of the command so that users don't see the installer UI. For example: 
```
msixmgr.exe -AddPackage C:\SomeDirectory\notepadplus.msix
```
### Querying for a specific MSIX Package
Searching for a specific package is possible by packageFullName, packageFamilyName and/or using wildcards as well. Supported wilcards are *(match any character) and ?(match single character). -
```
msixmgr.exe -FindPackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe
msixmgr.exe -FindPackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe
msixmgr.exe -FindPackage *padplus_0.0.*
msixmgr.exe -FindPackage *adplus_8wekyb3d8bbw?
```
### Uninstall
The -quietUX parameter can also be used here.
```
msixmgr.exe -RemovePackage notepadplus_0.0.0.1_x64__8wekyb3d8bbwe
```

The following commands depend on ApplyACLs.dll and msix.dll. Please place these next to msixmgr.exe.

### Unpacking
Extracts contents of .msix into a folder. Folder will be named according to the package full name of the package and will be placed in the given destination directory. The -applyacls option can be optionally specified to apply ACLs to the resulting folder.
```
msixmgr.exe -Unpack -packagepath C:\SomeDirectory\notepadplus.msix -destination C:\output [-applyacls]
```

> [!NOTE]
> The commands above uses **notepadplus.msix** is one of our [sample packages](https://github.com/microsoft/msix-packaging/tree/master/preview/MsixCore/Tests).

## Creating MSIX Core MSIX from source code
### Prerequisite
In order to create the MSIX Core MSI project in Visual Studio, the WiX Toolset and WiX Visual Studio Extensions must be [installed](https://wixtoolset.org/releases/). 

### Build your MSIX Core MSI 
Clone the msix-packaging repository to a local workspace and build it x64 (see documentation on [MSIX SDK](https://github.com/Microsoft/msix-packaging for prerequisites) using the -mt flag, which is necessary to avoid vclibs dependency; this should output the msix.dll, which is consumed by MSIXCore project.

```
Makewin.cmd x64 -mt
```
Open the msix-packaging/preview/MsixCore/msixmgr.sln file in Visual Studio. Build the msixmgr project in release/x64 to create the msixmgr.exe

## Using a MSI Setup Project
Once the msixmgr project has been built, the MsixMgrWix project can be built. The MsixMgrWix project has an additional dependency on the GetMsixmgrProducts project, which builds a custom action for the MSI package.

The msixmgrSetup Project creates a .msi package to deploy the msix.dll and msixmgr.exe onto a Windows 7 SP1 or higher machine. The MSI Setup Project will register the specific file type association for the .msix and .appx extensions such that the installer is initiated directly from double-clicking a MSIX or APPX package.

