---
author: diahar
title: MSIX Modification packages on Windows 10 1809 update 
description: In this section, we will review modification packages in Windows 10 1809 Update
ms.author: diahar
ms.date: 10/19/2018
ms.topic: article
ms.prod: windows
ms.technology: windows 10, uwp, msix
keywords: 
ms.localizationpriority: medium
---

# MSIX Modification packages on Windows 10 1809 update 
With the Windows 10 1908 update, you can now package your registry-based plugins and customization inside an MSIX modification package. Customization can include configurations needed to be set in the registry such that the main package will run as expected. Meaning your main application leverages the registry to view whether a plugin exist. When you deploy the main package and the modification package, at runtime the application will view the virtual registry (VREG) of both the main package and the modification package. 

Note that your main package may be using the VREG to do the following things: 
1.	Viewing where to load the file i.e. dll of plugin. If this is the case, then ensure that the file is part of the package. By doing this, main package is able to access the file at runtime.  
2.	Viewing where to see the value of the VREG keys. Your main package may be looking for a value to exist in the VREG. When you create your modification package either by hand or using our tool, ensure that the value is correct. 

You can create a modification package with the MSIX Packaging Tool 
-	Specify the main package. Be sure to have the MSIX version of your main package available on your machine that you are converting on. If not than we will ask you to manually provide the publisher and main application information. Also some customization require that your main application is installed on your machine.
-	Modify the package ones it has gone through conversion using the package editor. There may be a case where the main package requires your modification package to have certain values in their VREG. This is where you would go and edit the package appropriately. 

You can create a modification package manually by using the makeappx tool that you can get with our Windows 10 SDK
-	In the manifest, specify the main package – include the publisher and the main package name

```xml

<Dependencies>
  <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVErsionTest="12.0.0.0"/>
  <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
</Dependencies>
```
•	Create Registry.dat, User.dat and Userclass.dat to whatever is needed to load your modification package. This is only required if you need your main application to view custom registry keys. Remember that since everything is running inside a container, at runtime the main package and the modification package virtual registry will merge such that main package can view the modification packages virtual registry.  

We also support file system type plugins and customization if the executable of the main application is not in a VFS. This is to ensure that the main package will get all the VFS of the main package and the modification package. We are currently working on supporting file system type plugins and customization with executable in VFS in the next release. 

Thanks for taking the time to read. 

-MSIX team
