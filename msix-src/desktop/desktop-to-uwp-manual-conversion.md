---
title: Generating MSIX package components
description: Shows how to manually package a Windows desktop application (such as Win32, WPF, or Windows Forms) for Windows.
ms.date: 07/04/2026
ms.topic: how-to
keywords: windows 11, windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.custom: RS5
---

# Generating MSIX package components

This article shows you how to generate MSIX package components for packaging your application using command line tools (without using Visual Studio or the MSIX Packaging Tool).

To manually package your app, you need to create a package manifest file, add your package components and then run the **MakeAppx.exe** command line tool to generate an MSIX package.

## First, prepare to package

If you haven't yet, review this section on [what you need to know before packaging your application](../desktop/before-packaging-overview.md).

## Create a package manifest

Create a file, name it **appxmanifest.xml**, and then add this XML to it.

It's a basic template that contains the elements and attributes that your package needs. We'll add values to these in the next section.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
  <Properties>
    <DisplayName></DisplayName>
    <PublisherDisplayName></PublisherDisplayName>
    <Description></Description>
    <Logo></Logo>
  </Properties>
  <Resources>
    <Resource Language="" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application Id="" Executable=""
      uap10:RuntimeBehavior="packagedClassicApp"
      uap10:TrustLevel="mediumIL">
      <uap:VisualElements DisplayName="" Description=""	Square150x150Logo=""
        Square44x44Logo="" BackgroundColor="" />
    </Application>
  </Applications>
</Package>
```

> [!NOTE]
> If your package installs on systems older than Windows 10, version 2004 (10.0; Build 19041), then use the `EntryPoint` attribute instead of `uap10:RuntimeBehavior` and `uap10:TrustLevel`. For more details, and examples, see [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041).

## Fill in the package-level elements of your file

Fill in this template with information that describes your package.

### Identity information

Here's an example **Identity** element with placeholder text for the attributes. You can set the ``ProcessorArchitecture`` attribute to ``x64`` , ``x86`` , ``arm`` (i.e. 32bit ARM), ``arm64`` , or ``neutral``

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
			    ProcessorArchitecture="x64">
```
> [!NOTE]
> If you've reserved your application name in the Microsoft Store, you can obtain the Name and Publisher by using [Partner Center](https://partner.microsoft.com/dashboard). If you plan to sideload your application onto other systems, you can provide your own names for these as long as the publisher name that you choose matches the name on the certificate you use to sign your app.

### Properties

The [Properties](/uwp/schemas/appxpackage/appxmanifestschema/element-properties) element has 3 required child elements. Here is an example **Properties** node with placeholder text for the elements. The **DisplayName** is the name of your application that you reserve in the Store, for apps which are uploaded to the Store.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### Resources

Here is an example [Resources](/uwp/schemas/appxpackage/appxmanifestschema/element-resources) node.

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### Dependencies

For desktop apps that you create a package for, always set the ``Name`` attribute to ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### Capabilities

For a package that contains one or more full-trust apps, you'll need to declare the `runFullTrust` restricted capability as shown below. For full details, and a definition of *full-trust app*, search for *Full Trust Permission Level* in [App capability declarations](/windows/uwp/packaging/app-capability-declarations)).

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```

## Fill in the application-level elements

Fill in this template with information that describes your app.

### Application element

For desktop apps that you create a package for, configure the **Application** element like this:

```XML
<Applications>
  <Application Id="MyApp" Executable="MyApp.exe"
		 uap10:RuntimeBehavior="packagedClassicApp"
     uap10:TrustLevel="mediumIL">
   </Application>
</Applications>
```

> [!NOTE]
> If your package installs on systems older than Windows 10, version 2004 (10.0; Build 19041), then use the `EntryPoint` attribute instead of `uap10:RuntimeBehavior` and `uap10:TrustLevel`. For more details, and examples, see [uap10 was introduced in Windows 10, version 2004 (10.0; Build 19041)](/uwp/schemas/appxpackage/uapmanifestschema/element-application#uap10-was-introduced-in-windows-10-version-2004-100-build-19041).

### Visual elements

Here is an example [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) node.

```XML
<uap:VisualElements
	BackgroundColor="#464646"
	DisplayName="My App"
	Square150x150Logo="images\icon.png"
	Square44x44Logo="images\small_icon.png"
	Description="A useful description" />
```
<a id="target-based-assets"></a>

## (Optional) Add Target-based unplated assets

Target-based assets are for icons and tiles that appear on the Windows taskbar, task view, ALT+TAB, snap-assist, and the lower-right corner of Start tiles. You can read more about them [here](/windows/apps/design/style/app-icons-and-logos#unplated-assets).

1. Obtain the correct 44x44 images and then copy them into the folder that contains your images (i.e., Assets).

2. For each 44x44 image, create a copy in the same folder and append **.targetsize-44_altform-unplated** to the file name. You should have two copies of each icon, each named in a specific way. For example, after completing the process, your assets folder might contain **MYAPP_44x44.png** and **MYAPP_44x44.targetsize-44_altform-unplated.png**.

   > [!NOTE]
   > In this example, the icon named **MYAPP_44x44.png** is the icon that you'll reference in the ``Square44x44Logo`` logo attribute of your MSIX package.

3. In the manifest file,  set the ``BackgroundColor`` for every icon you are making transparent.

4. Continue to the next subsection to generate a new Package Resource Index file.

<a id="make-pri"></a>

### Generate a Package Resource Index (PRI) file using MakePri

If you create target-based assets as described in the section above, or you modify any of the visual assets of your application after you've created the package, you'll have to generate a new PRI file.

Based on your installation path of the SDK, this is where **MakePri.exe** is on your Windows PC:
- x86: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x86\makepri.exe
- x64: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x64\makepri.exe

There is no ARM version of this tool.

1.	Open a Command Prompt or PowerShell window.

2.  Change directory to the package's root folder, and then create a priconfig.xml file by running the command ``<path>\makepri.exe createconfig /cf priconfig.xml /dq en-US``.

5.	Create the resources.pri file(s) by using the command ``<path>\makepri.exe new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    For example, the command for your application might look like this: ``<path>\makepri.exe new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.	Package your application by using the instructions in the next step.

<a id="make-appx"></a>

<a id="virtual-registry"></a>

## (Optional) Include registry entries (virtual registry)

If your application relies on registry keys under *HKLM\Software*, you can ship those keys inside the package as a *virtual registry*. Windows stores a package's virtual registry in a `Registry.dat` hive file at the root of the package layout. At runtime, the OS merges that hive over *HKLM\Software* so the app sees its keys without writing to the real system registry. For details on how the virtual registry behaves at runtime, see [the Registry section of Understanding how packaged desktop apps run on Windows](desktop-to-uwp-behind-the-scenes.md#registry).

> [!IMPORTANT]
> **MakeAppx.exe** packages the contents of your layout folder as-is&mdash;it doesn't generate `Registry.dat` from a `.reg` file. If your package needs a virtual registry, a valid `Registry.dat` hive must already be present in the layout folder before you run MakeAppx. If your app doesn't need any packaged registry keys, you can omit the file entirely.

There are a few supported ways to produce the `Registry.dat` hive, depending on your workflow:

- **Automate it in a CI/CD pipeline (recommended).** Use the MSIX Packaging Tool's command line interface to run an unattended conversion. As it monitors your installer, the tool captures the installer's registry writes and generates the `Registry.dat` hive as part of the finished package. See [Create a package using the command line interface](../packaging-tool/package-conversion-command-line.md) and [Generate a template file](../packaging-tool/generate-template-file.md) for driving conversions from a script.
- **Edit an existing package.** Open the package in the MSIX Packaging Tool's Package Editor and use the [Virtual registry page](../packaging-tool/package-editor.md#virtual-registry-page) to add, modify, or remove packaged registry entries, then save and re-sign the package.
- **Generate a hive directly (advanced).** The built-in `reg save` command writes the contents of a registry key to a hive file in the same binary format as `Registry.dat`. Populate a temporary key with the values your app needs (for example, by importing a `.reg` file into a scratch location), run `reg save` to produce the hive, and add it to your layout. Because the hive represents *HKLM\Software*, verify the result by opening the package in the Package Editor's Virtual registry page before you ship it.

## Test your application before packaging

You can deploy your unpackaged application and test it before packaging or signing. To do so, run the cmdlet below from a PowerShell window. Make sure to pass in your application's manifest file located in the root of your package directory with all your other package components:

```Add-AppxPackage –Register AppxManifest.xml```

Once this is done. Your app should be deployed on the system and you can test it to make sure everything works before packaging. To update your app's .exe or .dll files, replace the existing files in your package with the new ones, increase the version number in AppxManifest.xml, and then run the above command again.

## Package your components into an MSIX

The next step is to use **MakeAppx.exe** to generate an MSIX package for your application. Makeappx.exe is included with the Windows SDK, and if you have Visual Studio installed, it can be easily accessed through the Developer Command Prompt for Visual Studio.

See [Create an MSIX package or bundle with the MakeAppx.exe tool](../package/create-app-package-with-makeappx-tool.md)

> [!NOTE]
> A packaged application always runs as an interactive user, and any drive that you install your packaged application on to must be formatted to NTFS format.
