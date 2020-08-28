---
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
title: Package an application manually (Desktop Bridge)
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
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
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""	Square150x150Logo=""
				   Square44x44Logo=""	BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## Fill in the package-level elements of your file

Fill in this template with information that describes your package.

### Identity information

Here's an example **Identity** element with placeholder text for the attributes. You can set the ``ProcessorArchitecture`` attribute to ``x64`` or ``x86``.

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
For desktop apps that you create a package for, you'll have to add the ``runFullTrust`` capability.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## Fill in the application-level elements

Fill in this template with information that describes your app.

### Application element

For desktop apps that you create a package for, the ``EntryPoint`` attribute of the Application element is always ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
		Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

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

Target-based assets are for icons and tiles that appear on the Windows taskbar, task view, ALT+TAB, snap-assist, and the lower-right corner of Start tiles. You can read more about them [here](/windows/uwp/design/style/app-icons-and-logos#unplated-assets).

1. Obtain the correct 44x44 images and then copy them into the folder that contains your images (i.e., Assets).

2. For each 44x44 image, create a copy in the same folder and append **.targetsize-44_altform-unplated** to the file name. You should have two copies of each icon, each named in a specific way. For example, after completing the process, your assets folder might contain **MYAPP_44x44.png** and **MYAPP_44x44.targetsize-44_altform-unplated.png**.

   > [!NOTE]
   > In this example, the icon named **MYAPP_44x44.png** is the icon that you'll reference in the ``Square44x44Logo`` logo attribute of your MSIX package.

3. In the manifest file,  set the ``BackgroundColor`` for every icon you are making transparent.

4. Continue to the next subsection to generate a new Package Resource Index file.

<a id="make-pri"></a>

### Generate a Package Resource Index (PRI) file using MakePri

If you create target-based assets as described in the section above, or you modify any of the visual assets of your application after you've created the package, you'll have to generate a new PRI file.

Based on your installation path of the SDK, this is where **MakePri.exe** is on your Windows 10 PC:
- x86: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x86\makepri.exe
- x64: C:\Program Files (x86)\Windows Kits\10\bin\\&lt;build number&gt;\x64\makepri.exe

There is no ARM version of this tool.

1.	Open a Command Prompt or PowerShell window.

2.  Change directory to the package's root folder, and then create a priconfig.xml file by running the command ``<path>\makepri.exe createconfig /cf priconfig.xml /dq en-US``.

5.	Create the resources.pri file(s) by using the command ``<path>\makepri.exe new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    For example, the command for your application might look like this: ``<path>\makepri.exe new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.	Package your application by using the instructions in the next step.

<a id="make-appx"></a>

## Test your application before packaging

You can deploy your non-packaged application and test it before packaging or signing. To do so, run the cmdlet below from a PowerShell window. Make sure to pass in your application's manifest file located in the root of your package directory with all your other package components:

```Add-AppxPackage –Register AppxManifest.xml```

Once this is done. Your app should be deployed on the system and you can test it to make sure everything works before packaging. To update your app's .exe or .dll files, replace the existing files in your package with the new ones, increase the version number in AppxManifest.xml, and then run the above command again.

## Package your components into an MSIX

The next step is to use **MakeAppx.exe** to generate an MSIX package for your application. Makeappx.exe is included with the Windows 10 SDK, and if you have Visual Studio installed, it can be easily accessed through the Developer Command Prompt for Visual Studio.

See [Create an MSIX package or bundle with the MakeAppx.exe tool](../package/create-app-package-with-makeappx-tool.md)



> [!NOTE]
> A packaged application always runs as an interactive user, and any drive that you install your packaged application on to must be formatted to NTFS format.