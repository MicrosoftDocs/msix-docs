---
description: This article describes the end-to-end process for creating a modern Windows 10 app package for your existing Windows Forms, WPF, or Win32 app or game. 
title: Package desktop applications
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
---

# Package desktop applications (MSIX)

Take your existing desktop application and add modern experiences for Windows 10 users. Then, achieve greater reach across international markets by distributing it through the Microsoft Store. You can monetize your application in much simpler ways by leveraging features built right into the Store. Of course, you don't have to use the Store. Feel free to use your existing channels.

When you create an MSIX package for your desktop application, your application will get an identity and with that identity, your desktop application has access to Windows Universal Platform (UWP) APIs. You can use them to light up modern and engaging experiences such as live tiles and notifications. Use simple conditional compilation and runtime checks to run UWP code only when your application runs on Windows 10.

Aside from the code that you use to light up Windows 10 experiences, your application remains unchanged and you can continue to distribute it to users on previous versions of Windows. On Windows 10, your application continues to run in full-trust user mode just like it’s doing today.

## Prepare

First, prepare your application by reviewing the article [Prepare to package your desktop app](desktop-to-uwp-prepare.md), and then addressing any of the issues that apply to your application before you create a Windows app package for it. You might not have to make many changes to your application before you create the package. However, there are some situations that might require you to tweak your application before you create a package for it.

<a id="convert" />

## Package

There are several different ways to create an MSIX package for your desktop app.

### Build an MSIX from an existing app installer

If you already have an app package (for example, an MSI or App-V Installer), we recommend that you use the [MSIX Packaging Tool](../mpt-overview.md) to repackage your existing desktop app to the MSIX format. It offers both an interactive UI and a command line for conversions, and gives you the ability to convert an application without having the source code. 

An earlier tool named the [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) is also still available for repackaging an existing desktop app package. However, this tool is now deprecated, and we recommend that you use the MSIX Packaging Tool instead.

The following table shows the supported versions of Windows 10 and package formats for these tools.

|  Tool  | Supported OS versions for creating packages  | Supported OS versions for installed packages  |  Supported package formats  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  [MSIX Packaging Tool](../mpt-overview.md)        |  Windows 10, version 1809 and later           | Windows 10, version 1709 and later            |  .msix only                 |
|  [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)        |  Windows 10, version 1607 and later           | Windows 10, version 1607 and later            |  .appx only    |

To get started using the MSIX Packaging Tool, see these articles:

* [Create an MSIX package from a desktop installer (MSI, EXE or App-V) on a VM](../packaging-tool/create-app-package-msi-vm.md)
* [Create an MSIX package with all other installer types](../packaging-tool/create-other-installer.md)
* [Create an MSIX package using the Command Line](../packaging-tool/package-conversion-cli.md)

### Build an MSIX from source code using Visual Studio

If you maintain your application by using Visual Studio, and your application doesn't have an installer or your installer doesn't perform too many complicated tasks, consider using Visual Studio instead.

Visual Studio makes it easy to create a package. You'll add a **Windows Application Package Project** to your solution, reference your desktop project, and then press F5 to debug your app. Here's a few other things you can do with it.

:heavy_check_mark: Automatically generate visual assets.

:heavy_check_mark: Make changes to your manifest by using a visual designer.

:heavy_check_mark: Generate your package by using a wizard.

:heavy_check_mark: Easily assign an identity to your application from a name that you've already reserved in [Partner Center](https://partner.microsoft.com/dashboard).

For instructions, see [Package a desktop application by using Visual Studio](desktop-to-uwp-packaging-dot-net.md). The following table shows the supported versions of Visual Studio, Windows 10, and package formats.

|  Supported versions of Visual Studio | Supported OS versions for creating packages  | Supported OS versions for installed packages  |  Supported package formats  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 and later       |  Windows 10, version 1607 and later           |  Windows 10, version 1607 and later            |  .msix (for Windows 10, version 1709 and later)<br/>.appx (for Windows 10, version 1607 and later)                 |

### Third-party installers

Several popular third-party products and installers now support the ability to package a desktop application. For more details and a list of these third-party products, see [Package a desktop application using third-party installers](desktop-to-uwp-third-party-installer.md).

### Manual packaging

As a final option, you can convert your application without using any of these tools. If you want that granular control over your conversion, you can create a manifest file, and then run the **MakeAppx.exe** tool to create your Windows app package.

See [Package a desktop application manually](desktop-to-uwp-manual-conversion.md).

## Add modern Windows 10 experiences

After you create an MSIX package for your desktop app, you can use UWP APIs, package extensions, and UWP components to light up modern and engaging Windows 10 experiences such as live tiles and notifications.

### Enhance with UWP APIs

Once you've packaged your app, you can light it up with features such as live tiles, and push notifications. Some of these capabilities can significantly improve the engagement level of your application and they cost you very little time to add. Some enhancements require a bit more code.

See [Use UWP APIs in desktop applications](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### Integrate with package extensions

If your application needs to integrate with the system (For example: establish firewall rules), describe those things in the package manifest of your application and the system will do the rest. For most of these tasks, you won't have to write any code at all. With a bit of XML in the manifest, you can do things like start a process when the user logs on, integrate your application into File Explorer, and add your application a list of print targets that appear in other apps.

See [Integrate your desktop application with package extensions](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### Extend with UWP components

Some Windows 10 experiences (For example: a touch-enabled UI page) must run inside of a modern app container. In general, you should first determine whether you can add your experience by [Enhancing](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) your existing desktop application with UWP APIs. If you have to use a UWP component, to achieve the experience, then you can add a UWP project to your solution and use app services to communicate between your desktop application and the UWP component.

See [Extend your desktop application with UWP components](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

## Test

To test your application in a realistic setting as you prepare for distribution, it's best to sign your application and then install it. See [Test your app](desktop-to-uwp-debug.md#test-your-app).

>[!IMPORTANT]
> If you plan to publish your application to the Microsoft Store, make sure that your application operates correctly on devices that run Windows 10 in S mode. This is a Store requirement. See [Test your Windows app for Windows 10 in S mode](desktop-to-uwp-test-windows-s.md).

## Validate

To give your application the best chance of being published on the Microsoft Store or becoming [Windows Certified](https://go.microsoft.com/fwlink/p/?LinkID=309666), validate and test it locally before you submit it for certification.

If you're using the DAC to package your app, you can use the new ``-Verify`` flag to validate your package against the packaged desktop application and Store requirements. See [Package an app, sign the app, and prepare it for Store submission](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

If you're using Visual Studio, you can validate your application from the **Create App Packages** wizard. See [Create an app package upload file](../package/packaging-uwp-apps.md#create-an-app-package-upload-file).

To run the tool manually, see [Windows App Certification Kit](/windows/uwp/debug-test-perf/windows-app-certification-kit).

To review the list of tests that the Windows App Certification uses to validate your app, see [Windows Desktop Bridge app tests](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests).

## Distribute

You can distribute your application by publishing it the Microsoft Store or by sideloading it onto other systems.

See [Distribute a packaged desktop app](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).

## Find answers to your questions

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
