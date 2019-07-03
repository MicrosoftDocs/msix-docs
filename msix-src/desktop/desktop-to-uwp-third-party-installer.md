---
Description: This guide provides a list of third-party products and installers to package desktop applications.
title: Package a desktop app using third-party installers
ms.date: 06/17/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Package a desktop app using third-party installers

Below is a list of popular third-party products and installers that support the ability to package a desktop application. You can use them to generate MSI installers or app packages with only a few clicks. While we don't produce documentation on how to use these tools, visit their websites to learn more.

## Advanced Installer

Caphyon provides a free, GUI-based, desktop app packaging tool that helps you to generate a Windows app package for your application with only a few clicks. It can use any installer; even ones that run in silent mode, and performs a validation check to determine whether the application is suitable for packaging. The Desktop App Converter also integrates with Hyper-V and [VMware](https://www.vmware.com/). This means that you can use your own virtual machines, without having to download a matching [Docker](https://docs.docker.com/) image that can be over 3GB in size.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

You can use [Advanced Installer](https://www.advancedinstaller.com/) to generate MSI and [Windows app packages](https://www.advancedinstaller.com/uwp-app-package.html) from existing projects. You can also use Advanced installer to import Windows app packages that you generate by using the Microsoft Desktop App Converter. Once imported, you can maintain them by using visual tools that are specifically designed for UWP apps.

Advanced Installer also provides an extension for Visual Studio 2017 and 2015 that can use to [build and debug Desktop Bridge apps](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

See this [video](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) for a quick overview.

> [!TIP]
> Be sure to checkout the recently released [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

## Cloudhouse Compatibility Containers

For Enterprise customers who have line of business applications that are incompatible with Windows 10 and 10 S, Cloudhouse’s Compatibility Containers enable Windows XP and 7 apps to run on Windows 10 and then converts them to run on the Universal Windows Platform (UWP) for delivery through Microsoft Store for Business, or Microsoft InTune without changing the source code. Register for a [Free Trial](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse provides an Auto Packager for packaging line of business applications into [Compatibility Containers](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) on the operating systems that the apps runs on today (For example: Windows XP), and then [prepare it for conversion](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) to UWP. The Container is then converted to the new Windows app package format by integrating it with Microsoft’s Desktop App Converter tool.

The Auto Packager uses install / capture and runtime analysis to create a Container for the application which includes the application’s files, registry, runtimes, dependencies, and the compatibility and redirection engine required to enable the application to run on Windows 10. The Container provides isolation for the application and its runtimes, so that that they do not affect or conflict with other applications running on the user’s device.

Find out more about how you can deliver business applications through the Microsoft Store for Business Read in our [Release blog](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## FireGiant

The [FireGiant MSIX extension](https://www.firegiant.com/products/wix-expansion-pack/msix) lets you create Windows app packages and MSI packages simultaneously from the same WiX source code. Every time you build, you can target Windows 10 with a Windows app package and earlier versions of Windows with MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

The FireGiant MSIX extension uses static analysis and intelligent emulation of your WiX projects to create Windows app packages without the disk space and runtime overhead of containers or virtual machines.

Because the FireGiant MSIX extension doesn't convert your installer by running it, you can maintain your WiX installer without having to repeatedly convert it to Windows app packages. All your users on different versions of Windows get your latest improvements and you don't have to worry about MSI and Windows app packages getting out of sync.

Check out this [video](https://www.youtube.com/watch?v=AFBpdBiAYQE) and see how in a couple lines of code FireGiant CEO Rob Mensching creates an Appx (Windows app package) version of the popular open-source 7-Zip compression tool and then how he improves both Windows application and MSI packages with changes in the same WiX source code.

## InstallAware

InstallAware, with a [track record](https://www.installaware.com/press-room.htm) of quickly supporting Microsoft's innovations, builds [Windows app packages (Desktop Bridge)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer), and EXE (Native Code) packages from a single source.

<img width="20%" src="images/installaware.png">

InstallAware provides free InstallAware extensions for Visual Studio versions 2012-2017. You can use them to create Windows app packages with a single click directly from the [Visual Studio toolbar](https://www.installaware.com/visual-studio-installer-2015.htm).

You can also import any setup, even if you don't have the source code for that setup, by using PackageAware (snapshot-free setup captures), or the Database Import Wizard (for all MSI installers and MSM merge modules). You can use [GUI tools](https://www.installaware.com/scripting-two-way-integrated-ide.htm) to maintain and enhance your imports, visually or by scripting.

[Advanced APPX creation options](https://www.installaware.com/mhtml5/desktop/appx.htm) help you target Microsoft Store submissions, or produce signed Windows app package binaries for sideload distribution to end-users. You can even build WSA (Windows Server Applications) Installer packages that target deployments to **Nano Server** all from a single source, and with full support for [command line automation](https://www.installaware.com/scripting-automation-interface.htm), in addition to a GUI.

InstallAware also [open sourced](https://www.installaware.com/gnu.asp) an **APPX builder library**, together with an example command line applet, under the GNU Affero GPL license. These are designed for use with open source platforms such as WiX.

## InstallShield

InstallShield provides a single solution to develop MSI and EXE installers, create Universal Windows Platform (UWP) and Windows Server App (WSA) packages, and virtualize applications with minimal scripting, coding and rework.

<img width="20%" src="images/InstallShield-logo.jpg">

Scan your InstallShield project in seconds to save hours of investigative work by automatically identifying potential compatibility issues between your application and UWP and WSA packages.

Prepare for the Microsoft Store and simplify your software’s installation experience on Windows 10 by building UWP app packages from your existing InstallShield projects. Build both Windows Installer and UWP App Packages to support all of your customers’ desired deployment scenarios. Support Nano Server and Windows Server 2016 deployments by building WSA packages from your existing InstallShield projects.

Develop your installation in modules for easier deployment and maintenance, and then merge the components and dependencies at build time into a single UWP app package for the Microsoft Store. For direct distribution outside the Store, bundle your UWP App Packages and other dependencies together with a Suite/Advanced UI installer.

Learn more in this [eBook](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## PACE Suite

[PACE Suite](https://pacesuite.com/) is an application packaging tool that you can use to bring your desktop apps to the Universal Windows Platform.

<img width="20%" src="images/PACE.png">

With PACE Suite, you don't need to prepare special packaging environments or install additional Windows SDK components. PACE Suite can build Windows app packages independently in your standard packaging environment under Windows 10 or Windows Server 2016. Check out this [illustrated example](https://pacesuite.com/convert-exe-to-appx/) to learn how PACE Suite approaches repackaging an installer to a Windows app package.

Apart from creating Windows app packages, you can also use PACE Suite to create Windows Installer packages (MSI), patches (MSP), transforms (MST) and App-V packages. When it comes to MSI authoring, PACE Suite helps with managing upgrades, permission settings, custom actions, scripts and others. You can also publish your applications directly to System Center Configuration Manager.

To review all application packaging capabilities, see [PACE Suite features](https://pacesuite.com/features/).

## RAD Studio

See [RAD Studio by Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## RayPack Studio

Raynet's packaging solution, [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), supports the creation of packages for desktop applications as one of several possible outcomes of efficient and easy-to-configure conversion and repackaging framework.

<img width="20%" src="images/RaynetLogo_v3.png">

Existing virtual environments (VMware Workstation, Hyper-V) can be used to perform automated/bulk conversion without a lengthy environment setup. A component of the studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) is able to make pre-conversion screening and compatibility tests to verify software that is eligible for conversion. Additionally, users can now perform comprehensive collision and compatibility checks with various Windows 10 editions including Anniversary and Creators updates.

Next to the creation of software packages for Windows 10 APPX/UWP format, RayPack Studio can also be used to create classic Windows Installer packages (MSI), patches (MSP), transforms (MST), and App-V packages. Furthermore, this solution comes with a set of software products and components for professional enterprise software packaging. In addition to software packaging and virtualization, RayPack Studio considers all packaging-related tasks: conflict and compatibility checks of software applications and packages ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), software evaluation ([RayEval](https://raynet.de/Raynet-Products/RayEval)), and quality assurance ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combined with [RayFlow](https://raynet.de/Raynet-Products/RayFlow), Raynet´s Enterprise Workflow System, users can efficiently work on the software through the whole enterprise application lifecycle, from package ordering, through evaluation, analysis, packaging, quality assurance, user acceptance tests and deployment. All packages and formats can be stored and deployed directly into SCCM or other solutions. The entire application lifecycle process is tracked and managed by RayFlow. In addition, any order systems such as ServiceNow can be integrated. Raynet builds software packaging factories worldwide with its tools for service providers.

Convince yourself and get the [free trial license](https://raynet.de/contact?init=license) of Raynet's RayPack Studio and RayFlow. For more information, please visit [www.raynet.de](https://raynet.de/home).

Related links:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Free Trial License: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
