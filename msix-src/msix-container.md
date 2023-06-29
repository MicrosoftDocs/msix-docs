---
title: MSIX appContainer apps
description: This topic describes MSIX appContainer apps and how to configure a package for one.
ms.date: 06/29/2023
ms.topic: article
ms.author: stwhi
author: stevewhims
keywords: windows 11, windows 10, uwp, msix
--- 

# MSIX appContainer apps

If you have an app that's packaged using MSIX, then you can configure it to run in a lightweight app container. Universal Windows Platform (UWP) apps are *appContainer* apps. And you can also configure a desktop app (if it's packaged with MSIX) to be an *appContainer* app. An *appContainer* app's process and its child processes run inside the container; and they're isolated using file system and registry virtualization (all MSIX apps can *read* the global registry).

## The purpose of an app container

A key goal of an *appContainer* app is to separate app state from system state as much as possible, while maintaining compatibility with other apps. Windows accomplishes that by detecting and redirecting certain changes that it makes to the file system and registry at runtime (known as *virtualizing*).

An *appContainer* app writes to its own virtual registry and application data folder, and that data is deleted when the app is uninstalled or reset. Other apps don't have access to the virtual registry or virtual file system of an *appContainer* app.

## Configure a WinUI 3 project for appContainer

The steps in [Create a new project for a packaged C# or C++ WinUI 3 desktop app](/windows/apps/winui/winui3/create-your-first-winui3-app#packaged-create-a-new-project-for-a-packaged-c-or-c-winui-3-desktop-app) show the default and recommended way to create a new WinUI 3 project.

By default, the project's `Package.appxmanifest` file contains configuration for a full trust (that is, medium integrity level) package. The relevant sections look like this:

```xml
...
<Applications>
  <Application ...
    EntryPoint="$targetentrypoint$">
    ...
  </Application>
</Applications>

<Capabilities>
  <rescap:Capability Name="runFullTrust" />
</Capabilities>
...
```

To configure the package as containing an *appContainer* app, you can edit the **EntryPoint** attribute, and remove the restricted capability declaration (but keep the **Capabilities** element). Like this:

```xml
...
<Applications>
  <Application ...
    EntryPoint="windows.partialTrustApplication">
    ...
  </Application>
</Applications>

<Capabilities/>
...
```

If your package installs on Windows 10, version 2004 (10.0; Build 19041) and/or later, then instead of setting **EntryPoint**, you can set **uap10:TrustLevel** and **uap10:RuntimeBehavior** (after declaring the XML namespace prefix, as shown). Like this:

```xml
<Package ...
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  ...>
...
  <Applications>
    <Application ...
      EntryPoint="$targetentrypoint$"
      uap10:TrustLevel="appContainer"
      uap10:RuntimeBehavior="packagedClassicApp">
      ...
    </Application>
  </Applications>

  <Capabilities/>
...
```

For more info, see these topics:

* [Application element](/uwp/schemas/appxpackage/uapmanifestschema/element-application)
* [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041)
* [Types of desktop app](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#types-of-desktop-app)

## Configure a WinUI 3 two-project solution for appContainer

The previous section described the process for single-project MSIX; which we recommend, and which is the default for new WinUI 3 projects. For more info, see [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix).

But you might have a WinUI 3 project that dates from before the introduction of the single-project MSIX feature. In which case you'll have two projects in your solution&mdash;your app project, plus an additional **Windows Application Packaging Project**. If you can migrate your project to single-project MSIX, then that's ideal. And you'll be able to follow the guidance in the previous section. For more info, see [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix).

If you *can't* migrate your project to single-project MSIX, then this section describes how to configure your package as containing an *appContainer* app.

A **Windows Application Packaging Project** implies a default setting that overrides the configuration in `Package.appxmanifest`. The project behaves *as if* there were a **TrustLevel** property in the project file set to a value of *Full*.

To remedy that implied property value, expand the packaging project's **Dependencies** > **Applications** node, and select the node that represents the reference to your WinUI 3 project. Then in Visual Studio's **Properties** window (not project properties), for the **Trust Level** property pick the value of *Partial Trust*.

The project file for the packaging project now contains this explicit property:

```xml
...
<ItemGroup>
  <ProjectReference Include="..\MyWinUI3AppProject\MyWinUI3AppProject.vcxproj">
    <TrustLevel>Partial</TrustLevel>
  </ProjectReference>
</ItemGroup>
...
```

And you can now remove `<rescap:Capability Name="runFullTrust" />` from the packaging project's `Package.appxmanifest` file.

## Configure a WPF or WinForms project for appContainer

This section is for you if you have either:

* a [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/) app project that was created with the C# **WPF Application** project template. That will give you a .NET project; and it's different from the project template named **WPF App (.NET Framework)**. Or
* a [Windows Forms (WinForms)](/dotnet/desktop/winforms/) app project that was created with the C# **Windows Forms App** project template. That will give you a .NET project; and it's different from the project template named **Windows Forms App (.NET Framework)**.

The first step, in a nutshell, is to add to your solution a C# **Windows Application Packaging Project**. There are more details on the exact steps in [Set up your desktop application for MSIX packaging in Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

Then expand the packaging project's **Dependencies** > **Applications** node, and select the node that represents the reference to your WPF or WinForms project. Then in Visual Studio's **Properties** window (not project properties), for the **Trust Level** property pick the value of *Partial Trust*.

## Related topics

* [Create a new project for a packaged C# or C++ WinUI 3 desktop app](/windows/apps/winui/winui3/create-your-first-winui3-app#packaged-create-a-new-project-for-a-packaged-c-or-c-winui-3-desktop-app)
* [Application element](/uwp/schemas/appxpackage/uapmanifestschema/element-application)
* [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041)
* [Types of desktop app](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#types-of-desktop-app)
* [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix)
* [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/)
* [Windows Forms (WinForms)](/dotnet/desktop/winforms/)
* [Set up your desktop application for MSIX packaging in Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)
