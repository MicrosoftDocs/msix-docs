---
Description: Create a modern Windows app package for your existing Windows Forms, WPF, or Win32 app or game. Add modern experiences for Windows 10 users and simplify deployment and monetization.
title: Package desktop applications
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
---

# Package desktop applications (Desktop Bridge)

Take your existing desktop application and add modern experiences for Windows 10 users. Then, achieve greater reach across international markets by distributing it through the Microsoft Store. You can monetize your application in much simpler ways by leveraging features built right into the Store. Of course, you don't have to use the Store. Feel free to use your existing channels.

When you create a package for your desktop application, your application will get an identity and with that identity, your desktop application has access to Windows Universal Platform (UWP) APIs. You can use them to light up modern and engaging experiences such as live tiles and notifications. Use simple conditional compilation and runtime checks to run UWP code only when your application runs on Windows 10.

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

### Third-party installer

 Several popular third-party products and installers now support the ability to package a desktop application. You can use them to generate MSI installers or app packages with only a few clicks. While we don't produce documentation on how to use these tools, visit their websites to learn more.

#### Advanced Installer

Caphyon provides a free, GUI-based, desktop app packaging tool that helps you to generate a Windows app package for your application with only a few clicks. It can use any installer; even ones that run in silent mode, and performs a validation check to determine whether the application is suitable for packaging.
The Desktop App Converter also integrates with Hyper-V and [VMware](https://www.vmware.com/). This means that you can use your own virtual machines, without having to download a matching [Docker](https://docs.docker.com/) image that can be over 3GB in size.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

You can use [Advanced Installer](https://www.advancedinstaller.com/) to generate MSI and [Windows app packages](https://www.advancedinstaller.com/uwp-app-package.html) from existing projects. You can also use Advanced installer to import Windows app packages that you generate by using the Microsoft Desktop App Converter. Once imported, you can maintain them by using visual tools that are specifically designed for UWP apps.

Advanced Installer also provides an extension for Visual Studio 2017 and 2015 that can use to [build and debug Desktop Bridge apps](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

See this [video](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) for a quick overview.

> [!TIP]
> Be sure to checkout the recently released [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

#### Cloudhouse Compatibility Containers

For Enterprise customers who have line of business applications that are incompatible with Windows 10 and 10 S, Cloudhouse’s Compatibility Containers enable Windows XP and 7 apps to run on Windows 10 and then converts them to run on the Universal Windows Platform (UWP) for delivery through Microsoft Store for Business, or Microsoft InTune without changing the source code. Register for a [Free Trial](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse provides an Auto Packager for packaging line of business applications into [Compatibility Containers](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) on the operating systems that the apps runs on today (For example: Windows XP), and then [prepare it for conversion](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) to UWP. The Container is then converted to the new Windows app package format by integrating it with Microsoft’s Desktop App Converter tool.

The Auto Packager uses install / capture and runtime analysis to create a Container for the application which includes the application’s files, registry, runtimes, dependencies, and the compatibility and redirection engine required to enable the application to run on Windows 10. The Container provides isolation for the application and its runtimes, so that that they do not affect or conflict with other applications running on the user’s device.

Find out more about how you can deliver business applications through the Microsoft Store for Business Read in our [Release blog](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

#### FireGiant

The [FireGiant MSIX extension](https://www.firegiant.com/products/wix-expansion-pack/msix) lets you create Windows app packages and MSI packages simultaneously from the same WiX source code. Every time you build, you can target Windows 10 with a Windows app package and earlier versions of Windows with MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

The FireGiant MSIX extension uses static analysis and intelligent emulation of your WiX projects to create Windows app packages without the disk space and runtime overhead of containers or virtual machines.

Because the FireGiant MSIX extension doesn't convert your installer by running it, you can maintain your WiX installer without having to repeatedly convert it to Windows app packages. All your users on different versions of Windows get your latest improvements and you don't have to worry about MSI and Windows app packages getting out of sync.

Check out this [video](https://www.youtube.com/watch?v=AFBpdBiAYQE) and see how in a couple lines of code FireGiant CEO Rob Mensching creates an Appx (Windows app package) version of the popular open-source 7-Zip compression tool and then how he improves both Windows application and MSI packages with changes in the same WiX source code.

#### InstallAware

Install**Aware**, with a [track record](https://www.installaware.com/press-room.htm) of quickly supporting Microsoft's innovations, builds [Windows app packages (Desktop Bridge)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer), and EXE (Native Code) packages from a single source.

<img width="20%" src="images/installaware.png">

Install**Aware** provides free Install**Aware** extensions for Visual Studio versions 2012-2017. You can use them to create Windows app packages with a single click directly from the [Visual Studio toolbar](https://www.installaware.com/visual-studio-installer-2015.htm).

You can also import any setup, even if you don't have the source code for that setup, by using Package**Aware** (snapshot-free setup captures), or the Database Import Wizard (for all MSI installers and MSM merge modules). You can use [GUI tools](https://www.installaware.com/scripting-two-way-integrated-ide.htm) to maintain and enhance your imports, visually or by scripting.

[Advanced APPX creation options](https://www.installaware.com/mhtml5/desktop/appx.htm) help you target Microsoft Store submissions, or produce signed Windows app package binaries for sideload distribution to end-users. You can even build **WSA**(Windows Server Applications) Installer packages that target deployments to **Nano Server** all from a single source, and with full support for [command line automation](https://www.installaware.com/scripting-automation-interface.htm), in addition to a GUI.

Install**Aware** also [open sourced](https://www.installaware.com/gnu.asp) an **APPX builder library**, together with an example command line applet, under the GNU Affero GPL license. These are designed for use with open source platforms such as WiX.

#### InstallShield

InstallShield provides a single solution to develop MSI and EXE installers, create Universal Windows Platform (UWP) and Windows Server App (WSA) packages, and virtualize applications with minimal scripting, coding and rework.

<img width="20%" src="images/InstallShield-logo.jpg">

Scan your InstallShield project in seconds to save hours of investigative work by automatically identifying potential compatibility issues between your application and UWP and WSA packages.

Prepare for the Microsoft Store and simplify your software’s installation experience on Windows 10 by building UWP app packages from your existing InstallShield projects. Build both Windows Installer and UWP App Packages to support all of your customers’ desired deployment scenarios. Support Nano Server and Windows Server 2016 deployments by building WSA packages from your existing InstallShield projects.

Develop your installation in modules for easier deployment and maintenance, and then merge the components and dependencies at build time into a single UWP app package for the Microsoft Store. For direct distribution outside the Store, bundle your UWP App Packages and other dependencies together with a Suite/Advanced UI installer.

Learn more in this [eBook](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

#### PACE Suite

[PACE Suite](https://pacesuite.com/) is an application packaging tool that you can use to bring your desktop apps to the Universal Windows Platform.

<img width="20%" src="images/PACE.png">

With PACE Suite, you don't need to prepare special packaging environments or install additional Windows SDK components. PACE Suite can build Windows app packages independently in your standard packaging environment under Windows 10 or Windows Server 2016. Check out this [illustrated example](https://pacesuite.com/convert-exe-to-appx/) to learn how PACE Suite approaches repackaging an installer to a Windows app package.

Apart from creating Windows app packages, you can also use PACE Suite to create Windows Installer packages (MSI), patches (MSP), transforms (MST) and App-V packages. When it comes to MSI authoring, PACE Suite helps with managing upgrades, permission settings, custom actions, scripts and others. You can also publish your applications directly to System Center Configuration Manager.

To review all application packaging capabilities, see [PACE Suite features](https://pacesuite.com/features/).

#### RAD Studio

See [RAD Studio by Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### RayPack Studio

Raynet's packaging solution, [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), supports the creation of packages for desktop applications as one of several possible outcomes of efficient and easy-to-configure conversion and repackaging framework.

<img width="20%" src="images/RaynetLogo_v3.png">

Existing virtual environments (VMware Workstation, Hyper-V) can be used to perform automated/bulk conversion without a lengthy environment setup. A component of the studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) is able to make pre-conversion screening and compatibility tests to verify software that is eligible for conversion. Additionally, users can now perform comprehensive collision and compatibility checks with various Windows 10 editions including Anniversary and Creators updates.

Next to the creation of software packages for Windows 10 APPX/UWP format, RayPack Studio can also be used to create classic Windows Installer packages (MSI), patches (MSP), transforms (MST), and App-V packages. Furthermore, this solution comes with a set of software products and components for professional enterprise software packaging. In addition to software packaging and virtualization, RayPack Studio considers all packaging-related tasks: conflict and compatibility checks of software applications and packages ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), software evaluation ([RayEval](https://raynet.de/Raynet-Products/RayEval)), and quality assurance ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combined with [RayFlow](https://raynet.de/Raynet-Products/RayFlow), Raynet´s Enterprise Workflow System, users can efficiently work on the software through the whole enterprise application lifecycle, from package ordering, through evaluation, analysis, packaging, quality assurance, user acceptance tests and deployment. All packages and formats can be stored and deployed directly into SCCM or other solutions. The entire application lifecycle process is tracked and managed by RayFlow. In addition, any order systems such as ServiceNow can be integrated. Raynet builds software packaging factories worldwide with its tools for service providers.

Convince yourself and get the [free trial license](https://raynet.de/contact?init=license) of Raynet's RayPack Studio and RayFlow. For more information, please visit [www.raynet.de](https://raynet.de/home).

**Related links**:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Free Trial License: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

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

If you're using Visual Studio, you can validate your application from the **Create App Packages** wizard. See [Create an app package upload file](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps#create-an-app-package-upload-file).

To run the tool manually, see [Windows App Certification Kit](/windows/uwp/debug-test-perf/windows-app-certification-kit).

To review the list of tests that the Windows App Certification uses to validate your app, see [Windows Desktop Bridge app tests](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests).

## Distribute

You can distribute your application by publishing it the Microsoft Store or by sideloading it onto other systems.

See [Distribute a packaged desktop app](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).

## Support and feedback

**Find answers to your questions**

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Give feedback or make feature suggestions**

See [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
