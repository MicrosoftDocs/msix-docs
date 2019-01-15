---
author: dianmsft
title: MSIX Modification packages on Windows 10 1809 update
description: In this section, we will review modification packages in Windows 10 1809 Update
ms.author: diahar
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# MSIX modification packages on Windows 10 version 1809 

Starting with Windows 10 version 1809, you can now package your registry-based plugins and customization inside an [MSIX modification package](modification-packages.md). Customization can include configurations needed to be set in the registry such that the main package will run as expected. Meaning your main application leverages the registry to view whether a plugin exists. When you deploy the main package and the modification package, at runtime the application will view the virtual registry (VREG) of both the main package and the modification package. 

Note that your main package may be using the VREG to do the following things: 
1.	Viewing where to load the file i.e. dll of plugin. If this is the case, then ensure that the file is part of the package. By doing this, main package is able to access the file at runtime.  
2.	Viewing where to see the value of the VREG keys. Your main package may be looking for a value to exist in the VREG. When you create your modification package either by hand or using our [tool](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), ensure that the value is correct. 

## Create a modification package using the MSIX Packaging Tool

You can create a modification package with the MSIX Packaging Tool:
* Specify the main package. Be sure to have the MSIX version of your main package available on your machine that you are converting on. If not than we will ask you to manually provide the publisher and main application information. Also some customization require that your main application is installed on your machine.
![Modification Package MPT](images/MPT-mod-page.png)

* Modify the package once it has gone through conversion using the package editor. There may be a case where the main package requires your modification package to have certain values in their VREG. This is where you would go and edit the package appropriately. 

## Create a modification package using MakeAppx.exe

You can create a modification package manually by using the [MakeAppX.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool) tool that is included in the Windows 10 SDK:
* In the manifest, specify the main package. Include the publisher and the main package name.

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```
- Create Registry.dat, User.dat and Userclass.dat to create whatever registry keys are needed to load your modification package. This is only required if you need your main application to view custom registry keys. Remember that since everything is running inside a container, at runtime the main package and the modification package virtual registry will merge such that main package can view the modification packages virtual registry.  

This process also supports file system plug-ins and customizations, as long as the executable of the main application is not in a virtual file system (VFS). This is to ensure that the main package will get all the VFS of the main package and the modification package. 

Support for file system plug-ins and customizations while the executable of the main application is in a VFS is planned for the next major release of Windows. You can preview this support starting in Windows 10 Insider Preview Build 18312 or later. For more information, see [this article](modification-package-insider-preview-build-18312.md). 

