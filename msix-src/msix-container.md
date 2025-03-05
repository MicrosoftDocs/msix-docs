---
title: MSIX AppContainer apps
description: This topic shows how you can take an app that's packaged using MSIX, and easily configure it to run in the AppContainer environment (in a lightweight app container).
ms.date: 02/12/2025
ms.topic: article
keywords: windows 11, windows 10, winui3, uwp, msix
--- 

# MSIX AppContainer apps

The topic [AppContainer for legacy applications](/windows/win32/secauthz/appcontainer-for-legacy-applications-) covers all of the necessary background info about what the AppContainer environment is and its benefits; that topic also contains C# and C++ code examples for testing whether or not a process is running inside an AppContainer.

The topic you're reading now shows how you can take an app that's packaged using MSIX, and easily configure it to run in the AppContainer environment (in a lightweight app container). Universal Windows Platform (UWP) apps are automatically AppContainer apps. But you can also configure your desktop app that's packaged with MSIX to be an AppContainer app.

An AppContainer app's process and its child processes run inside a lightweight app container where they can access only the resources that are specifically granted to them. And they're isolated using file system and registry virtualization. As a result, apps implemented in an AppContainer can't be hacked to allow malicious actions outside of the limited assigned resources.

> [!TIP]
> Unpackaged apps can run in an AppContainer, too. But it's particularly easy to use AppContainer if you package using MSIX. So all of the scenarios described in this topic are about packaged apps.

## Configure a WinUI 3 project for AppContainer

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

To configure the package as containing an AppContainer app, you can edit the **EntryPoint** attribute, and remove the restricted capability declaration (but keep the **Capabilities** element). Like this:

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

## Configure a WinUI 3 two-project solution for AppContainer

The previous section described the process for single-project MSIX; which we recommend, and which is the default for new WinUI 3 projects. For more info, see [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix).

But you might have a WinUI 3 project that dates from before the introduction of the single-project MSIX feature. In which case you'll have two projects in your solution&mdash;your app project, plus an additional **Windows Application Packaging Project**. If you can migrate your project to single-project MSIX, then that's ideal. And you'll be able to follow the guidance in the previous section. For more info, see [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix).

If you *can't* migrate your project to single-project MSIX, then this section describes how to configure your package as containing an AppContainer app.

A **Windows Application Packaging Project** implies a default setting that overrides the configuration in `Package.appxmanifest`. The project behaves *as if* there were a **TrustLevel** property in the project file set to a value of *Full*.

To remedy that implied property value, expand the packaging project's **Dependencies** > **Applications** node, and select the node that represents the reference to your WinUI 3 project. Then in Visual Studio's **Properties** window (not project properties), for the **Trust Level** property pick the value of *Partial Trust*.

The project file for the packaging project now contains this explicit property:

```xml
...
<ItemGroup>
  <ProjectReference Include="...">
    <TrustLevel>Partial</TrustLevel>
  </ProjectReference>
</ItemGroup>
...
```

And you can now remove `<rescap:Capability Name="runFullTrust" />` from the packaging project's `Package.appxmanifest` file.

## Configure a Windows Application Project (C++ Win32 WndProc-type app) for AppContainer

This section is for you if you have a C++ Win32 WndProc-type project that was created with the **Windows Application Project** project template. The first step, in a nutshell, is to add to your solution a C++ **Windows Application Packaging Project**. There are more details on the exact steps in [Set up your desktop application for MSIX packaging in Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). That topic applies to desktop apps written in C++ or C#.

Then open your new packaging project's project file, and add a **TrustLevel** property to the existing **ProjectReference** property like this:

```xml
...
<ItemGroup>
  <ProjectReference Include="...">
    <TrustLevel>Partial</TrustLevel>
  </ProjectReference>
</ItemGroup>
...
```

When you build, you might see the error "error APPX1673: App manifest is missing required element 'PhoneIdentity'". If that happens, then edit the project's `Package.appxmanifest` file like this:

```xml
<Package ...
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  ...>
...
  <mp:PhoneIdentity
      PhoneProductId="A GUID in the form xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx."
      PhonePublisherId="A GUID in the form xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.">
  </mp:PhoneIdentity>
...
```

## Configure a WPF or WinForms project for AppContainer

This section is for you if you have either:

* a [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/) app project that was created with the C# **WPF Application** project template. That will give you a .NET project; and it's different from the project template named **WPF App (.NET Framework)**. Or
* a [Windows Forms (WinForms)](/dotnet/desktop/winforms/) app project that was created with the C# **Windows Forms App** project template. That will give you a .NET project; and it's different from the project template named **Windows Forms App (.NET Framework)**.

The first step, in a nutshell, is to add to your solution a C# **Windows Application Packaging Project**. There are more details on the exact steps in [Set up your desktop application for MSIX packaging in Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

Then expand the packaging project's **Dependencies** > **Applications** node, and select the node that represents the reference to your WPF or WinForms project. Then in Visual Studio's **Properties** window (not project properties), for the **Trust Level** property pick the value of *Partial Trust*.

## Related topics

* [AppContainer for legacy applications](/windows/win32/secauthz/appcontainer-for-legacy-applications-)
* [Create a new project for a packaged C# or C++ WinUI 3 desktop app](/windows/apps/winui/winui3/create-your-first-winui3-app#packaged-create-a-new-project-for-a-packaged-c-or-c-winui-3-desktop-app)
* [Application element](/uwp/schemas/appxpackage/uapmanifestschema/element-application)
* [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041)
* [Types of desktop app](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#types-of-desktop-app)
* [Package your app using single-project MSIX](/windows/apps/windows-app-sdk/single-project-msix)
* [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/)
* [Windows Forms (WinForms)](/dotnet/desktop/winforms/)
* [Set up your desktop application for MSIX packaging in Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)
