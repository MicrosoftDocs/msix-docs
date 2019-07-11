---
title: MSIX Modification packages on Windows 10 version 1903
description: In this section, we will review modification packages in Windows 10 1903 Update
ms.date: 01/14/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# MSIX modification packages on Windows 10 version 1903
 
In Windows 10 version 1809, we introduced [MSIX modification packages](modification-packages.md) that allow enterprises to customize their apps on Windows 10. In the next major release of Windows, we are adding additional support to allow IT professionals to package customizations such as file-based plug-ins into an MSIX package. 

The following features were added to Windows 10, version 1903.

## Manifest update
We’ve added support for the following element to the MSIX modification package’s manifest.

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

To ensure that modification packages work in version 1903 or later, the modification package's manifest must include this element. This will be done for you if you package your MSIX modification package using the January release of the MSIX packaging tool. If you've converted a package using our tool prior to the release, you can edit your existing package in our tool to add this new element. In addition, if users install the modification package, they will be alerted that the package may modify the main application.

If you are using a modification package that was created before version 1903, it is necessary to edit the package manifest to update the MaxVersionTested attribute to 10.0.18362.0.

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

## Overriding a file in the main package
You can override a file in the main package with a modification package. This does not mean you are changing the file of the main package. The file remains unchanged by the modification package. However, at runtime the main package sees both its files and the modification package’s file, and it will choose the modification package’s files to load. 

> [!NOTE]
> The file that the modification package intends to override must be in a virtual file system (VSF) folder. 

## File system based plug-in
You can package your file system base plug-in as a MSIX modification package. If your main packages load their plug-in by looking at a folder, you can install your main package and modification package separately. At runtime, the plug-in will appear because the main app can query its folders and the modification’s folders. 

> [!NOTE]
> The folder that the main application uses to load plug-ins must be in a virtual file system (VFS) folder.  

## What remains the same
Virtual registry plug-ins that have been converted into MSIX modification packages will still be supported in the next major release of Windows. 

