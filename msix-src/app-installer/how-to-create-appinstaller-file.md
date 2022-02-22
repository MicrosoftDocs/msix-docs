---
title: Create an App Installer file manually
description: This article describes how to install a related set via App Installer, including how to create a *.appinstaller file that defines your related set.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.custom: "RS5, seodec18"
---

# Create an App Installer file manually

This article shows how to manually create an App Installer file that defines a [related set](../package/optional-packages.md) with autoupdating and repairing capabilities. A related set is not one entity, but rather a combination of a main package and optional packages. 

To be able to install a related set as one entity, we must be able to specify the main package and optional package as one. To do this, we will need to create an XML file with an **.appinstaller** extension to define a related set. App Installer consumes the ***.appinstaller** file and allows the user to install all of the defined packages with a single click. 

During deployment, the App Installer file will:
- The Windows app Package referenced in the `URI` attribute of the < MainPackage > element will validate the `Name`, `Publisher` and `Version` of the target Windows app Package attributes. If the [Package/Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the Windows app Package manifest do not match, the installation will fail.
- Create a reference to the Update and Repair URIs for the package's family.


## How to create an App Installer file

To distribute your related set as one entity, you must create an App Installer file that contains the elements that are required by that [app installer schema](/uwp/schemas/appinstallerschema/app-installer-file).

1. [Create the *.AppInstaller file.](#step-1-create-the-appinstaller-file)
1. [Specify the App Installer file attributes.](#step-2-add-the-basic-template)
1. [Specify the main Windows app Package.](#step-3-add-the-main-package-information)
1. [Specify the related set Optional Package.](#step-4-add-the-optional-packages)
1. [Specify the dependency Windows app Framework Package.](#step-5-add-dependencies)
1. [Specify the Update URI paths.](#step-6-add-update-setting)
1. [Specify the Repair URI paths.](#step-7-add-auto-update-settings)
1. [Specify the Update Settings.](#step-8-add-auto-repair-settings)


##### Example of an App Installer file

Following the steps provided above, you will have successfully created an App Installer file which resembles the following:

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

    <UpdateURIs>
        <UpdateURI>http://mywebservice.azurewebsites.net/appset.appinstaller</UpdateURI>
        <UpdateURI>http://mywebservice2.azurewebsites.net/appset.appinstaller</UpdateURI>
    </UpdateURIs>

    <RepairURIs>
        <RepairURI>http://mywebservice.azurewebsites.net/appset.appinstaller</RepairURI>
        <RepairURI>http://mywebservice2.azurewebsites.net/appset.appinstaller</RepairURI>
    </RepairURIs>

    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="0"/>   
    </UpdateSettings>

</AppInstaller>
```


## Step 1: Create the *.appinstaller file
Using a text editor (Notepad.exe), create a new file with a filename extension of ***.AppInstaller**

##### How to:
1. Open the **Start Menu**.
1. Type the following: `notepad.exe`.
1. Open the **File** Menu.
1. Select **Save As** from the drop-down menu.


## Step 2: Add the basic template
Include the `AppInstaller` element into your App Installer file noting the version, path and network location of your App Installer file. The information in the `AppInstaller` element will be consumed when installing the associated Windows apps.

| Element     | Description                                                                |
|-------------|----------------------------------------------------------------------------|
| **xmlns**   | The XML Namespace                                                          |
| **Version** | The Version of the App Installer file in a quad-dotted notation (1.0.0.0). |
| **URI**     | A URI path to the current App Installer file, accessible by the device.    |


##### How to:
1. Open the file created in **Step 1**.
1. Copy the following XML content into your ***.AppInstaller** file.
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <AppInstaller
        xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
        Version=""
        Uri="" >
    </AppInstaller>
    ```

1. Update the `Version` attribute with the version of your App Installer file 
1. Update the `URI` attribute with the network location where this ***.AppInstaller** file will be accessible from.


## Step 3: Add the main package information
The `<MainPackage>` and `<MainBundle>` are used to identify the primary Windows app that will be installed using the App Installer file. The `<MainPackage>` is used when the Windows app installer is either a **.msix*, or **.appx*. Use the `<MainBundle>` when the Windows app installer is a bundled Windows app Installer, with an extension of **.msixbundle*, or **.appxbundle*.


| Element               | Description                                                                                                                                                                                             |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                  | The name of the primary application that is being distributed to through the App Installer file. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Name`.     |
| Publisher             | The canonical name of the publisher certificate used to sign the primary Windows app installer. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Publisher`. |
| Version               | The version of the primary Windows app installer in a quad-dotted notation (1.0.0.0). This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Version`.             |
| ProcessorArchitecture | The Architecture that the primary Windows app installer is installing on to.                                                                                                                            |
| URI                   | The URI path to the primary Windows app installation media.                                                                                                                                             |

The information in the `<MainBundle>` or `<MainPackage>` attribute should match the [Package/Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the app bundle manifest or app package manifest respectively.


##### Windows app Installer
If the main app package is an .msix or .appx file, then use `<MainPackage>`, as shown below. Be sure to include the ProcessorArchitecture, as it is mandatory for non-bundle packages.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
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

##### Windows app Bundle Installer
If the main app package is an .msixbundle or .appxbundle or file, then use the `<MainBundle>` in place of `<MainPackage>` as shown below. For bundles, ProcessorArchitecture is not required.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

</AppInstaller>
```

## Step 4: Add the optional packages
Similar to the main app package attribute, if the optional package can be either an app package or an app bundle, the child element within the `<OptionalPackages>` attribute should be `<Package>` or `<Bundle>` respectively. The package information in the child elements should match the identity element in the bundle or package manifest.

| Element               | Description                                                                                                                                                                                              |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                  | The name of the optional application that is being distributed to through the App Installer file. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Name`.     |
| Publisher             | The canonical name of the publisher certificate used to sign the optional Windows app installer. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Publisher`. |
| Version               | The version of the optional Windows app installer in a quad-dotted notation (1.0.0.0). This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Version`.             |
| ProcessorArchitecture | The Architecture that the optional Windows app installer is installing on to.                                                                                                                            |
| URI                   | The URI path to the primary Windows app installation media.                                                                                                                                              |


``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
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

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x64"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.msix" />

    </OptionalPackages>

</AppInstaller>
```

## Step 5: Add dependencies
In the dependencies element, you can specify the required framework packages for the main package or the optional packages.

| Element               | Description                                                                                                                                                                                                |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                  | The name of the dependency application that is being distributed to through the App Installer file. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Name`.     |
| Publisher             | The canonical name of the publisher certificate used to sign the dependency Windows app installer. This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Publisher`. |
| Version               | The version of the dependency Windows app installer in a quad-dotted notation (1.0.0.0). This can be found by running the following PowerShell cmdlet: `$(Get-AppxPackage [AppName]).Version`.             |
| ProcessorArchitecture | The Architecture that the dependency Windows app installer is installing on to.                                                                                                                            |
| URI                   | The URI path to the dependency Windows app installation media.                                                                                                                                             |

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <Dependencies>
        <Package 
            Name="Microsoft.VCLibs.140.00" 
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
            Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package 
            Name="Microsoft.VCLibs.140.00" 
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
            Version="14.0.24605.0" 
            ProcessorArchitecture="x64" 
            Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

## Step 6: Add Update setting

The App Installer file can also specify update setting so that the related sets can be automatically updated when a newer App Installer file is published. **\<UpdateSettings\>** is an optional element. Within  **\<UpdateSettings\>** the OnLaunch option specifies that update checks should be made on app launch, and HoursBetweenUpdateChecks="12" specifies that an update check should be made every 12 hours. If HoursBetweenUpdateChecks is not specified, the default interval used to check for updates is 24 hours. Additional types of updates, like background updates can be found in the Update Settings [schema](/uwp/schemas/appinstallerschema/element-update-settings); Additional types of on-launch updates like updates with a prompt can be found in the OnLaunch [schema](/uwp/schemas/appinstallerschema/element-onlaunch)

| Elements                  | Description                                                                                                                                                                   |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HoursBetweenUpdateChecks  | Defines the minimal gap in Windows app update checks.                                                                                                                         |
| UpdateBlocksActivation    | Defines the experience when an app update is checked for.                                                                                                                     |
| ShowPrompt                | Defines if a window is displayed when updates are being installed, and when updates are being checked for.                                                                    |
| ForceUpdateFromAnyVersion | Specifies that the next version of the application could be to a newer or older version. If True, will all for both, if False (default), only new versions will be installed. |

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <UpdateSettings>
        <OnLaunch 
            HoursBetweenUpdateChecks="12"
            UpdateBlocksActivation="true"
            ShowPrompt="true" />
        <AutomaticBackgroundTask />
        <ForceUpdateFromAnyVersion>true</ForceUpdateFromAnyVersion>
    </UpdateSettings>

</AppInstaller>
```

## Step 7: Add Auto Update Settings
>[!Important]
>The following settings are only available when using the 2021 schema on a Windows Insider build of Windows 10.

Windows apps installed with an App Installer file will default to updating their Windows app from the App Installer URI, adhereing to the configurations set in the previous step. The Update URIs configured in this step will act as fallback URIs that can be used if the original App Installer URI is no longer accessible. A maximum of 10 Update URI's can be configured for any Windows app.

The Update URI's must target App Installer files.

>[!Note]
> These settings only work when the schema is configured as 2021 or newer.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <UpdateSettings>
        <OnLaunch 
            HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

    <UpdateUris>
        <UpdateUri>https://www.contoso.com/Installers/MainApp.AppInstaller</UpdateUri>
        <UpdateUri>\\ServerName\Share\Installers\MainApp.AppInstaller</UpdateUri>
    </UpdateUris>

</AppInstaller>
```

## Step 8: Add Auto Repair Settings
>[!Important]
>The following settings are only available when using the 2021 schema on a Windows Insider build of Windows 10.


Windows apps installed on a device can support automatic repairing of the Windows app when it has become tampered with. The source installer that will be used to repair the Windows app can be configured using the `<RepairURIs>` property. The Windows app will attempt to repair itself based on the App Installer URI, if inaccessible, then the Windows app will use the Repair URI's to identify a repair source. A maximum of 10 Repair URI's can be configured for any Windows app.

The Repair URI's can target Windows app's or App Installer files. This setting does not require that the Windows app have been installed using an App Installer file.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2021"
    Version="1.0.0.0"
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" >

    <MainBundle
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.msixbundle" />

    <UpdateSettings>
        <OnLaunch 
            HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

    <RepairUris>
        <RepairUri></RepairUri>
        <RepairUri></RepairUri>
    </RepairUris>

</AppInstaller>
```

For all of the details on the XML schema, see [App Installer file reference](/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> The App Installer file type is new in Windows 10, version 1709 (the Windows 10 Fall Creators Update). There is no support for deployment of Windows 10 apps using an App Installer file on previous versions of Windows 10. The **HoursBetweenUpdateChecks** element is available starting in Windows 10, version 1803.
