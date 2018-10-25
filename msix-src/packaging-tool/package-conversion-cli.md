---
title: create package using command line interface | Microsoft Docs
description: create msix app package using command line interface 
author: laurenhughes
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
---

# Conversion with CLI

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>
      
To create a new MSIX package for your application, run the MsixPackagingTool.exe create-package command in a Command prompt window. 

Here are the parameters that can be passed as command line arguments:

|**Parameter** |	**Description**|
|---------|---------|
|-? --help	|Show help information|
|--template	| [required] path to the conversion template XML file containing package information and settings for this conversion|
|--virtualMachinePassword	| [optional] The password for the Virtual Machine to be used for the conversion environment. Notes: The template file must contain a VirtualMachine element and the Settings::AllowPromptForPassword attribute must not be set to true.|

Examples:

``` command prompt

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

**Conversion template file**

``` xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018">

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
	EnforceMicrosoftStoreVersioningRequirements="false">

        <ExclusionItems>
            <FileExclusion ExcludePath="[{CryptoKeys}]" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Crypto" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Search\Data" />
            <FileExclusion ExcludePath="[{Cookies}]" />
            <FileExclusion ExcludePath="[{History}]" />
            <FileExclusion ExcludePath="[{Cache}]" />
            <FileExclusion ExcludePath="[{Personal}]" />
            <FileExclusion ExcludePath="[{Profile}]\Local Settings" />
            <FileExclusion ExcludePath="[{Profile}]\NTUSER.DAT.LOG1" />
            <FileExclusion ExcludePath="[{Profile}]\ NTUSER.DAT.LOG2" />
            <FileExclusion ExcludePath="[{Recent}]" />
            <FileExclusion ExcludePath="[{Windows}]\debug" />
            <FileExclusion ExcludePath="[{Windows}]\Logs\CBS" />
            <FileExclusion ExcludePath="[{Windows}]\Temp" />
            <FileExclusion ExcludePath="[{Windows}]\WinSxS\ManifestCache" />
            <FileExclusion ExcludePath="[{Windows}]\WindowsUpdate.log" />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\$Recycle.Bin " />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\System Volume Information" />
            <FileExclusion ExcludePath="[{AppData}]\Microsoft\AppV" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Antimalware" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Windows Defender" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Windows Defender" />
            <FileExclusion ExcludePath="[{Local AppData}]\Temp" />

            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Cryptography" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Antimalware Setup" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\Microsoft Security Client" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Microsoft Antimalware" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\StreamMRU" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Streams" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\MACHINE\SOFTWARE\Wow6432Node\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Microsoft\AppV" />
            <RegistryExclusion ExcludePath= "REGISTRY\USER\[{AppVCurrentUserSID}]\Software\Wow6432Node\Microsoft\AppV" />
        </ExclusionItems>
    </Settings>


    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService ="true"/>
    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute -->

    <SaveLocation
    PackagePath="C:\users\user\Desktop\MyPackage.msix" 
    TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        Arguments="/quiet"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />

    <VirtualMachine Name="vmname" Username="vmusername" />

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">

        <Applications>
            <Application
                Id="MyApp1"
                Description="MyApp"
                DisplayName="My App"
                ExecutableName="MyApp.exe"/>
        </Applications>

        <Capabilities>
            <Capability Name="runFullTrust" />
        </Capabilities>

    </PackageInformation>
</MsixPackagingToolTemplate>
```

**Conversion template parameter reference**

Here is the complete list of parameters that you can use in the Conversion template file.

|**ConversionSettings** |	**Description** |
|---------|---------|
|Settings:: AllowTelemetry |		[optional] Enables telemetry logging for this invocation of the tool.|
|Settings:: ApplyAllPrepareComputerFixes	 |	[optional] Applies all recommended prepare computer fixes. Cannot be set when other attributes are used.|
|Settings:: GenerateCommandLineFile	 |	[optional] Copies the template file input to the SaveLocation directory for future use.|
|Settings:: AllowPromptForPassword |		[optional] Instructs the tool to prompt the user to enter passwords for the Virtual Machine and for the signing certificate if it is required and not specified.|
|Settings:: EnforceMicrosoftStoreVersioningRequirements |		[optional] Instructs the tool to enforce the package versioning scheme required for deployment from Microsoft Store and Microsoft Store for Business.|
|ExclusionItems |		[optional] 0 or more FileExclusion or RegistryExclusion elements. All FileExclusion elements must appear before any RegistryExclusion elements.|
|ExclusionItems::FileExclusion |		[optional] A file to exclude for packaging.|
|ExclusionItems::FileExclusion::ExcludePath |		Path to file to exclude for packaging.|
|ExclusionItems::RegistryExclusion |		[optional] A registry key to exclude for packaging.|
|ExclusionItems::RegistryExclusion:: ExcludePath |		Path to registry to exclude for packaging.|
|PrepareComputer::DisableDefragService |		[optional] Disables Windows Defragmenter while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService |		[optional] Disables Windows Search while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService	 |	[optional] Disables SMS Host while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService	 |	[optional] Disables Windows Update while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|SaveLocation     |[optional] An element to specify the save location of the tool. If not specified, the package will be saved under the Desktop folder.         |
|SaveLocation::PackagePath     |[optional] The path to the file or folder where the resulting MSIX package is saved.         |
|SaveLocation::TemplatePath    |[optional] The path to the file or folder where the resulting CLI template is saved.    |
|Installer::Path |		The path to the application installer.|
|Installer::Arguments |		The arguments to pass to the installer. You must pass the arguments to force your installer to run unattended/silently.|
|Installer::InstallLocation |		[optional] The full path to your application's root folder for the installed files if it were installed (e.g. "C:\Program Files (x86)\MyAppInstalllocation").|
|VirtualMachine |		[optional] An element to specify that the conversion will be run on a local Virtual Machine.|
|VrtualMachine::Name	 |	The name of the Virtual Machine to be used for the conversion environment.|
|VirtualMachine::Username |		[optional] The user name for the Virtual Machine to be used for the conversion environment.|
|PackageInformation::PackageName |		The Package Name for your MSIX package.|
|PackageInformation::PackageDisplayName |		The Package Display Name for your MSIX package.|
|PackageInformation::PublisherName |		The Publisher for your MSIX package.|
|PackageInformation::PublisherDisplayName |		The Publisher Display Name for your MSIX package.|
|PackageInformation::Version |		The version number for your MSIX package.|
|PackageInformation:: MainPackageNameForModificationPackage |		[optional] The Package identity name of the main package name. This is used when creating a modification package that takes a dependency on a main (parent) application.|
|Applications |		[optional] 0 or more Application elements to configure the Application entries in your MSIX package.|
|Application::Id |		The App ID for your MSIX application. This ID will be used for the Application entry detected that matches the specified ExecutableName. You can have multiple Application ID for executables in the package|
|Application::ExecutableName |		The executable name for the MSIX application that will be added to the package manifest. The corresponding application entry will be ignored if no application with this name is detected.|
|Application::Description |		[optional] The App Description for your MSIX application. If not used, the Application DisplayName will be used. This description will be used for the application entry detected that matches the specified ExecutableName|
|Application::DisplayName	 |	The App Display Name for your MSIX package. This Display Name will be used for the application entry detected that matches the specified ExecutableName|
|Capabilities |		[optional] 0 or more Capability elements to add custom capabilities to your MSIX package. “runFullTrust” capability is added by default during conversion.|
|Capability::Name |	The capability to add to your MSIX package.

<br>
<br>

<div class="container centered pageFooter">
        <h2>Have feedback for us? We'd love to hear it.</h2>
        <ul class="links">
           <li>
                <a href="mailto:MSIXWebsiteFeedback@service.microsoft.com" data-linktype="external">
                    Email the MSIX team
                </a>
            </li>
           
        </ul>
		</div>
<!--
 <div class="container centered pageFooter">
        <h2>Keep in touch with us</h2>
        <ul class="links">
           <li>
                <a href="https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX">
                    MSIX tech community
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/MSIX-PackageSupportFramework/issues">
                    Package Support Framework
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/msix-packaging/issues">
                    MSIX SDK
                </a>
            </li>
            <li>
                <a href="https://twitter.com/#!/search/realtime/%23msix">
                    Twitter
                </a>
            </li>
        </ul>
    </div>
</div>
-->
