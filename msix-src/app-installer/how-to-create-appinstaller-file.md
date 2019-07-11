---
title: Create an App Installer file manually
description: This article describes how to install a related set via App Installer. We will also go through the steps to construct a *.appinstaller file that will define your related set.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Create an App Installer file manually

This article shows how to manually create an App Installer file that defines a [related set](install-related-set.md). A related set is not one entity, but rather a combination of a main package and optional packages. 

To be able to install a related set as one entity, we must be able to specify the main package and optional package as one. To do this, we will need to create an XML file with an **.appinstaller** extension to define a related set. App Installer consumes the **.appinstaller** file and allows the user to install all of the defined packages with a single click. 

## App Installer file example

Before we go in to more detail, here is a complete sample msixbundle *.appinstaller file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix"
            ProcessorArchitecture="x64" />

    </OptionalPackages>
    
    <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0"/>   
  </UpdateSettings>

</AppInstaller>
```

During deployment, the App Installer file is validated against the app packages referenced in the `Uri` element. So, the `Name`, `Publisher` and `Version` should match the [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the app package manifest.

## How to create an App Installer file

To distribute your related set as one entity, you must create an App Installer file that contains the elements that are required by that [appinstaller schema](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### Step 1: Create the *.appinstaller file
Using a text editor, create a file (which will contain XML) and name it &lt;filename&gt;.appinstaller

### Step 2: Add the basic template
The basic template includes the App Installer file information.
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >
</AppInstaller>
```

### Step 3: Add the main package information

If the main app package is an .msix or .appx file, then use `<MainPackage>`, as shown below. Be sure to include the ProcessorArchitecture, as it is mandatory for non-bundle packages.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainPackage
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        ProcessorArchitecture="x64"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msix" />

</AppInstaller>
```

If the main app package is an .msixbundle or .appxbundle or file, then use the `<MainBundle>` in place of `<MainPackage>` as shown below. For bundles, ProcessorArchitecture is not required.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

</AppInstaller>
```

The information in the `<MainBundle>` or `<MainPackage>` attribute should match the [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the app bundle manifest or app package manifest respectively.

### Step 4: Add the optional packages
Similar to the main app package attribute, if the optional package can be either an app package or an app bundle, the child element within the `<OptionalPackages>` attribute should be `<Package>` or `<Bundle>` respectively. The package information in the child elements should match the identity element in the bundle or package manifest.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x64"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

</AppInstaller>
```

### Step 5: Add dependencies
In the dependencies element, you can specify the required framework packages for the main package or the optional packages.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### Step 6: Add Update setting

The App Installer file can also specify update setting so that the related sets can be automatically updated when a newer App Installer file is published. **<UpdateSettings>** is an optional element. Within  **<UpdateSettings>** the OnLaunch option specifies that update checks should be made on app launch, and HoursBetweenUpdateChecks="12" specifies that an update check should be made every 12 hours. If HoursBetweenUpdateChecks is not specified, the default interval used to check for updates is 24 hours. Additional types of updates, like background updates can be found in the Update Settings [schema](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/element-update-settings); Additional types of on-launch updates like updates with a prompt can be found in the OnLaunch [schema](https://docs.microsoft.com/en-us/uwp/schemas/appinstallerschema/element-onlaunch)

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.msixbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.msixbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

For all of the details on the XML schema, see [App Installer file reference](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> The App Installer file type is new in Windows 10, version 1709 (the Windows 10 Fall Creators Update). There is no support for deployment of Windows 10 apps using an App Installer file on previous versions of Windows 10. The **HoursBetweenUpdateChecks** element is available starting in Windows 10, version 1803.
