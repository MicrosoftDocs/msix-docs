---
Description: This article lists things you need to know before packaging your desktop application. You may not need to do much to get your app ready for the packaging process.
title: Prepare to package a desktop application (MSIX)
ms.date: 08/22/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
---

# Prepare to package a desktop application

This article lists the things you need to know before you package your desktop application. You might not have to do much to get your application ready for the packaging process, but if any of the items below apply to your application, you need to address it before packaging.

+ __Your .NET application requires a version of the .NET Framework earlier than 4.6.2__. If you are packaging a .NET application, we recommend that your application target .NET Framework 4.6.2 or later. The ability to install and run packaged desktop applications was first introduced in Windows 10, version 1607 (also called the Anniversary Update), and this OS version includes the .NET Framework 4.6.2 by default. Later OS versions include later versions of the .NET Framework. For a full list of what versions of .NET are included in later versions of Windows 10, see [this article](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies).

  Targeting versions of the .NET Framework earlier than 4.6.2 in packaged desktop applications is expected to work in most cases. However, if you target an earlier version than 4.6.2, you should fully test your packaged desktop application before distributing it to users.

  + 4.0 - 4.6.1: Applications that target these versions of the .NET Framework are expected to run without issues on 4.6.2 or later. Therefore, these applications should install and run without changes on Windows 10, version 1607 or later with the version of the .NET Framework that is included with the OS.

  + 2.0 and 3.5: In our testing, packaged desktop applications that target these versions of the .NET Framework generally work but may exhibit performance issues in some scenarios. In order for these packaged applications to install and run, the [.NET Framework 3.5 feature](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10) must be installed on the target machine (this feature also includes .NET Framework 2.0 and 3.0). You should also test these applications thoroughly after you package them.

+ __Your application always runs with elevated security privileges__. Your application needs to work while running as the interactive user. Users who install your application may not be system administrators, so requiring your application to run elevated means that it won't run correctly for standard users. If you plan on publishing your app to the Mcirosoft Store, apps that require elevation for any part of their functionality won't be accepted into the Store.

+ __Your application requires a kernel-mode driver or a Windows service__. The Desktop Bridge is suitable for an app, but it does not support a kernel-mode driver or a Windows service that needs to run under a system account. Instead of a Windows service, use a [background task](/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Your app's modules are loaded in-process to processes that are not in your Windows app package__. This isn't permitted, which means that in-process extensions, like [shell extensions](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), aren't supported. But if you have two apps in the same package, you can do inter-process communication between them.

+ __Ensure that any extensions installed by the application will install where the application is installed__. Windows allows users and IT managers to change the default install location for packages.  See "Settings->System->Storage->More Storage Settings-> Change where new content is saved to -> New Apps will save to".  If you are installing an extension with your application, make sure that the extension does not have additional installation folder restrictions.  For example, some extensions may disable installing their extenion to non-system drives.  This will result in an error 0x80073D01 (ERROR_DEPLOYMENT_BLOCKED_BY_POLICY) if the default location has been changed. 

+ __Your application uses a custom Application User Model ID (AUMID)__. If your process calls [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) to set its own AUMID, then it may only use the AUMID generated for it by the application model environment/Windows app package. You can't define custom AUMIDs.

+ __Your application modifies the HKEY_LOCAL_MACHINE (HKLM) registry hive__. Any attempt by your application to create an HKLM key, or to open one for modification, will result in an access-denied failure. Remember that your application has its own private virtualized view of the registry, so the notion of a user- and machine-wide registry hive (which is what HKLM is) does not apply. You will need to find another way of achieving what you were using HKLM for, like writing to HKEY_CURRENT_USER (HKCU) instead.

+ __Your application uses a ddeexec registry subkey as a means of launching another app__. Instead, use one of the DelegateExecute verb handlers as configured by the various Activatable* extensions in your [app package manifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Your application writes to the AppData folder or to the registry with the intention of sharing data with another app__. After conversion, AppData is redirected to the local app data store, which is a private store for each app.

  All entries that your application writes to the HKEY_LOCAL_MACHINE registry hive are redirected to an isolated binary file and any entries that your application writes to the HKEY_CURRENT_USER registry hive are placed into a private per-user, per-app location. For more details about file and registry redirection, see [Behind the scenes of the Desktop Bridge](desktop-to-uwp-behind-the-scenes.md).  

  Use a different means of inter-process data sharing. For more info, see [Store and retrieve settings and other app data](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Your application writes to the install directory for your app__. For example, your application writes to a log file that you put in the same directory as your exe. This isn't supported, so you'll need to find another location, like the local app data store.

+ __Your application uses the current working directory__. At runtime, your packaged desktop application won't get the same working directory that you previously specified in your desktop .LNK shortcut. You need to change your CWD at runtime if having the correct directory is important for your application to function correctly.

  > [!NOTE]
  > If your app needs to write to the installation directory or use the current working directory, you can also consider adding a runtime fixup using the [Package Support Framework](https://github.com/microsoft/MSIX-PackageSupportFramework) to your package. For more details, see [this article](../psf/package-support-framework.md). 

+ __Your application installation requires user interaction__. Your application installer must be able to run silently, and it must install all of its prerequisites that aren't on by default on a clean OS image.

+ __Your application requires UIAccess__. If your application specifies `UIAccess=true` in the `requestedExecutionLevel` element of the UAC manifest, conversion to MSIX isn't supported currently. For more info, see [UI Automation Security Overview](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Your application exposes COM objects__. Processes and extensions from within the package can register and use COM & OLE servers, both in-process and out-of-process (OOP).  The Creators Update adds Packaged COM support which provides the ability to register OOP COM & OLE servers that are now visible outside the package.  See [COM Server and OLE Document support for Desktop Bridge](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge).

   Packaged COM support works with existing COM APIs, but will not work for application extensions that rely upon directly reading the registry, as the location for Packaged COM is in a private location.

+ __Your application exposes GAC assemblies for use by other processes__. In the current release, your application cannot expose GAC assemblies for use by processes originating from executables external to your Windows app package. Processes from within the package can register and use GAC assemblies as normal, but they will not be visible externally. This means interop scenarios like OLE will not function if invoked by external processes.

+ __Your application is linking C runtime libraries (CRT) in an unsupported manner__. The Microsoft C/C++ runtime library provides routines for programming for the Microsoft Windows operating system. These routines automate many common programming tasks that are not provided by the C and C++ languages. If your application utilizes C/C++ runtime library, you need to ensure it is linked in a supported manner.

	Visual Studio 2017 supports both static and dynamic linking, to let your code use common DLL files, or static linking, to link the library directly into your code, to the current version of the CRT. If possible, we recommend your application use dynamic linking with VS 2017.

	Support in previous versions of Visual Studio varies. See the following table for details:

	<table>
	<th>Visual Studio version</td><th>Dynamic linking</th><th>Static linking</th></th>
	<tr><td>2005 (VC 8)</td><td>Not supported</td><td>Supported</td>
	<tr><td>2008 (VC 9)</td><td>Not supported</td><td>Supported</td>
	<tr><td>2010 (VC 10)</td><td>Supported</td><td>Supported</td>
	<tr><td>2012 (VC 11)</td><td>Supported</td><td>Not supported</td>
	<tr><td>2013 (VC 12)</td><td>Supported</td><td>Not supported</td>
	<tr><td>2015 and 2017 (VC 14)</td><td>Supported</td><td>Supported</td>
	</table>

	Note: In all cases, you must link to the latest publicly available CRT.

+ __Your application installs and loads assemblies from the Windows side-by-side folder__. For example, your application uses C runtime libraries VC8 or VC9 and is dynamically linking them from Windows side-by-side folder, meaning your code is using the common DLL files from a shared folder. This is not supported. You will need to statically link them by linking to the redistributable library files directly into your code.

+ __Your application uses a dependency in the System32/SysWOW64 folder__. To get these DLLs to work, you must include them in the virtual file system portion of your Windows app package. This ensures that the application behaves as if the DLLs were installed in the **System32**/**SysWOW64** folder. In the root of the package, create a folder called **VFS**. Inside that folder create a **SystemX64** and **SystemX86** folder. Then, place the 32-bit version of your DLL in the **SystemX86** folder, and place the 64-bit version in the **SystemX64** folder.

+ __Your app uses a VCLibs framework package__. If you are converting a C++ Win32 app, you must handle the deployment of the Visual C++ Runtime. Visual Studio 2019 and the Windows SDK include the latest framework packages for version 11.0, 12.0 and 14.0 of the Visual C++ Runtime in the following folders:

    * **VC 14.0 framework packages**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop\14.0

    * **VC 12.0 framework packages**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.120\14.0

    * **VC 11.0 framework packages**: C:\Program Files (x86)\Microsoft SDKs\Windows Kits\10\ExtensionSDKs\Microsoft.VCLibs.Desktop.110\14.0

    To use one of these packages, you must reference the package as a dependency in your package manifest. When customers install the retail version of your app from the Microsoft Store, the package will be installed from the Store along with your app. The dependencies will not get installed if you side load your app. To install the dependencies manually, you must install the appropriate framework package using the appropriate .appx package for x86, x64, or ARM located in the installation folders listed above.

    To reference a Visual C++ Runtime framework package in your app:

    1. Go to the framework package install folder listed above for the version of the Visual C++ Runtime used by your app.

    2. Open the SDKManifest.xml file in that folder, locate the `FrameworkIdentity-Debug` or `FrameworkIdentity-Retail` attribute (depending on whether you're using the debug or retail version of the runtime), and copy the `Name` and `MinVersion` values from that attribute. For example, here's the `FrameworkIdentity-Retail` attribute for the current VC 14.0 framework package.
        ```xml
        FrameworkIdentity-Retail = "Name = Microsoft.VCLibs.140.00.UWPDesktop, MinVersion = 14.0.27323.0, Publisher = 'CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US'"
        ```

    3. In the package manifest for your app, add the following `<PackageDependency>` element under the `<Dependencies>` node. Make sure you replace the `Name` and `MinVersion` values with the values you copied in the previous step. The following example specifies a dependency for the current version of the VC 14.0 framework package.
        ```xml
        <PackageDependency Name="Microsoft.VCLibs.140.00.UWPDesktop" MinVersion="14.0.27323.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
        ```

+ __Your application contains a custom jump list__. There are several issues and caveats to be aware of when using jump lists.

	- __Your app's architecture does not match the OS.__  Jump lists currently do not function correctly if the application and OS architectures do not match (e.g., an x86 application running on x64 Windows). At this time, there is no workaround other than to recompile your application to the matching architecture.

	- __Your application creates jump list entries and calls [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) or [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Do not programmatically set your AppID in code. Doing so will cause your jump list entries to not appear. If your application needs a custom Id, specify it using the manifest file. Refer to [Package a desktop application manually](desktop-to-uwp-manual-conversion.md) for instructions. The AppID for your application is specified in the *YOUR_PRAID_HERE* section.

	- __Your application adds a jump list shell link that references an executable in your package__. You cannot directly launch executables in your package from a jump list (with the exception of the absolute path of an app’s own .exe). Instead, register an app execution alias (which allows your packaged desktop application to start via a keyword as though it were on the PATH) and set the link target path to the alias instead. For details on how to use the appExecutionAlias extension, see [Integrate your desktop application with Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions). Note that if you require assets of the link in jump list to match the original .exe, you will need to set assets such as the icon using [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) and the display name with PKEY_Title like you would for other custom entries.

	- __Your application adds a jump list entries that references assets in the app's package by absolute paths__. The installation path of an application may change when its packages are updated, changing the location of assets (such as icons, documents, executable, and so on). If jump list entries reference such assets by absolute paths, then the application should refresh its jump list periodically (such as on application launch) to ensure paths resolve correctly. Alternatively, use the UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) APIs instead, which allow you to reference string and image assets using the package-relative ms-resource URI scheme (which is also language, DPI, and high contrast aware).

+ __Your application starts a utility to perform tasks__. Avoid starting command utilities such as PowerShell and Cmd.exe. In fact, if users install your application onto a system that runs the Windows 10 S, then your application won’t be able to start them at all. This could block your application from submission to the Microsoft Store because all apps submitted to the Microsoft Store must be compatible with Windows 10 S.

Starting a utility can often provide a convenient way to obtain information from the operating system, access the registry, or access system capabilities. However, you can use UWP APIs to accomplish these sorts of tasks instead. Those APIs are more performant because they don’t need a separate executable to run, but more importantly, they keep the application from reaching outside of the package. The app’s design stays consistent with the isolation, trust, and security that comes with an application that you've packaged, and your application will behave as expected on systems running Windows 10 S.

+ __Your application hosts add-ins, plug-ins, or extensions__.   In many cases, COM-style extensions will likely continue to work as long as the extension has not been packaged, and it installs as full trust. That's because those installers can use their full-trust capabilities to modify the registry and place extension files wherever your host application expects to find them.

   However, if those extensions are packaged, and then installed as a Windows app package, they won't work because each package (the host application and the extension) will be isolated from one another. To read more about how applications are isolated from the system, see [Behind the scenes of the Desktop Bridge](desktop-to-uwp-behind-the-scenes.md).

 All applications and extensions that users install to a system running Windows 10 S must be installed as Windows App packages. So if you intend to package your extensions, or you plan to encourage your contributors to package them, consider how you might facilitate communication between the host application package and any extension packages. One way that you might be able to do this is by using an [app service](https://docs.microsoft.com/windows/uwp/launch-resume/app-services).

+ __Your application generates code__. Your application can generate code that it consumes in memory, but avoid writing generated code to disk because the Windows App Certification process can't validate that code prior to app submission. Also, apps that write code to disk won’t run properly on systems running Windows 10 S. This could block your application from submission to the Microsoft Store because all apps submitted to the Microsoft Store must be compatible with Windows 10 S.

>[!IMPORTANT]
> After you've created your Windows app package, please test your application to ensure that it works correctly on systems that run Windows 10 S. All apps submitted to the Microsoft Store must be compatible with Windows 10 S. Apps that aren't compatible won't be accepted in the store. See [Test your Windows app for Windows 10 S](desktop-to-uwp-test-windows-s.md).

## Next steps

**Create a Windows app package for your desktop app**

See [Create a Windows app package](desktop-to-uwp-root.md#convert)

**Find answers to your questions**

Have questions? Ask us on the [MSIX Tech Community forum](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX).

