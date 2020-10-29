---
title: Know your installer
description: This article lists things you need to know before packaging your desktop application. You may not need to do much to get your app ready for the packaging process.
ms.date: 01/29/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Know your installer

This article lists the things you need to know before you convert your existing installer into an MSIX. You might not have to do much to get your application ready for the packaging process, but if any of the items below applies to your application, you need to address it before packaging.

+ __Your application has a service__. We support converting [applications with services](convert-an-installer-with-services.md), but its important to keep in mind the [limitations](convert-an-installer-with-services.md#known-limitations) to converting a service. After conversion, you will need admin elevation in order to install the MSIX that contains a service. You can convert an application with services beginning in version 1.2019.1220.0 of the MSIX Packaging Tool, and you can deploy the MSIX with services beginning in the spring 2020 release of Windows 10.

+ __Your installer requires a restart__. If your installer requires a [restart](support-restart.md), this is supported in the MSIX Packaging Tool beginning in version 1.2019.701.0. If your installer returns an uncommon exit code to indicate it needs a restart, you should add it to the [restart exit codes](tool-best-practices.md#other-settings) section of the MSIX Packaging Tool settings. 

+ __Your .NET application requires a version of the .NET Framework earlier than 4.6.2__. If you are packaging a .NET application, we recommend that your application target .NET Framework 4.6.2 or later. The ability to install and run packaged desktop applications was first introduced in Windows 10, version 1607 (also called the Anniversary Update), and this OS version includes the .NET Framework 4.6.2 by default. Later OS versions include later versions of the .NET Framework. For a full list of what versions of .NET are included in later versions of Windows 10, see [this article](/dotnet/framework/migration-guide/versions-and-dependencies).

  Targeting versions of the .NET Framework earlier than 4.6.2 in packaged desktop applications is expected to work in most cases. However, if you target an earlier version than 4.6.2, you should fully test your packaged desktop application before distributing it to users.

  + 4.0 - 4.6.1: Applications that target these versions of the .NET Framework are expected to run without issues on 4.6.2 or later. Therefore, these applications should install and run without changes on Windows 10, version 1607 or later with the version of the .NET Framework that is included with the OS.

  + 2.0 and 3.5: In our testing, packaged desktop applications that target these versions of the .NET Framework generally work but may exhibit performance issues in some scenarios. In order for these packaged applications to install and run, the [.NET Framework 3.5 feature](/dotnet/framework/install/dotnet-35-windows-10) must be installed on the target machine (this feature also includes .NET Framework 2.0 and 3.0). You should also test these applications thoroughly after you package them.

+ __Your application requires a driver__. MSIX does not support drivers. 

+ __Your application writes to the AppData folder or to the registry with the intention of sharing data with another app__. After conversion, AppData is redirected to the local app data store, which is a private store for each app.

  All entries that your application writes to the HKEY_LOCAL_MACHINE registry hive are redirected to an isolated binary file and any entries that your application writes to the HKEY_CURRENT_USER registry hive are placed into a private per-user, per-app location. For more details about file and registry redirection, see [Behind the scenes of the Desktop Bridge](../desktop/desktop-to-uwp-behind-the-scenes.md). 

 + __Your application writes to the install directory for your app__. For example, your application writes to a log file that you put in the same directory as your exe. This isn't supported because the folder is protected. We recommend writing to another location like the local app data store. We have added a capability that allows this in 1809 and later.

+ __Your application uses the current working directory__. At runtime, your packaged desktop application won't get the same working directory that you previously specified in your desktop .LNK shortcut. You need to change your CWD at runtime if having the correct directory is important for your application to function correctly.

+ __Your application installs and loads assemblies from the Windows side-by-side folder__. For example, your application uses C runtime libraries VC8 or VC9 and is dynamically linking them from Windows side-by-side folder, meaning your code is using the common DLL files from a shared folder, such as C:\Windows\WinSxS. This is not supported. You will need to statically link them by linking to the redistributable library files directly into your code.

## Other considerations 

+ __Repackaging your installer on the proper architecture__. If your installer is intended to be installed on a x86 machine. Be sure to repackage your installer on a x86 machine. This is applicable for installer intended for x64 machines. 

  > [!NOTE]
  > If your app needs to write to the installation directory or use the current working directory, you can also consider adding a runtime fixup using the [Package Support Framework](https://github.com/microsoft/MSIX-PackageSupportFramework) to your package. For more details, see [this article](../psf/package-support-framework.md).
