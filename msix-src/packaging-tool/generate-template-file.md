---
title: How to generate a template file for command line conversions
description: This article describes how to generate a template file for command line conversions.
ms.date: 01/28/2020
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# How to generate a template file for command line conversions

With the MSIX Packaging Tool, you can perform the conversion in two ways: through the interactive UI or through our command line option. When using the command line, you need to provide a template file so that the conversion works with your specific settings and needs. This article will help guide you through the process of generating a template file that works for you.

There are two ways that you can get a template file that works for you:

* You can use the UI of the MSIX Packaging Tool. In the settings of the tool, you can specify that you want to generate a conversion template file with each MSIX package that you create.
* You can take a [sample template](#sample-conversion-template-file) and manually enter the configurations that you need for each conversion.

## Generate a conversion template file from the MSIX Packaging Tool

1. Launch the MSIX Packaging Tool.
2. Go to the settings in the top right corner of the application.
3. Make sure that the 'Generate a command line file with each package' option is selected.
4. Make any other changes or modifications to your settings that you need (e.g. exclusion items, exit codes).
5. Save the settings

6. Go through the Application Package workflow using an installer
    - If you don't select an installer, you will not be able to generate a conversion template file
    - If you are using an exe, you will need to pass a silent flag to your installer to generate the conversion template file
7. At the end of the conversion, you will have a template file configured based on the installer you chose, and your current settings which you can now re-use for future conversions
    - By default, the conversion template file will be saved in the same location as your MSIX package, but you can specify a separate save location for your template file on the Create package page
    - You will still need to make some modifications based on what MSIX you want output at the end of each conversion


## Manually edit the conversion template file

You can manually edit the template parameters for the conversion template file to generate a template file that works for you. When generating the conversion template file, pay attention to which features you add into the template file, as some may require an additional schema references in order to work.

### Conversion template parameter reference

Here is the complete list of parameters that you can use in the Conversion template file.

|**ConversionSettings** |	**Description** |
|---------|---------|
|Settings:: AllowTelemetry | [optional] Enables telemetry logging for this invocation of the tool.|
|Settings:: ApplyAllPrepareComputerFixes | [optional] Applies all recommended prepare computer fixes. Cannot be set when other attributes are used.|
|Settings:: GenerateCommandLineFile | [optional] Copies the template file input to the SaveLocation directory for future use.|
|Settings:: AllowPromptForPassword | [optional] Instructs the tool to prompt the user to enter passwords for the Virtual Machine and for the signing certificate if it is required and not specified.|
|Settings:: EnforceMicrosoftStoreVersioningRequirements | [optional] Instructs the tool to enforce the package versioning scheme required for deployment from Microsoft Store and Microsoft Store for Business.|
|Settings:: ServerPortNumber| [optional] Used when connecting to a remote machine. Requires v2 of the template schema.|
|Settings:: AddPackageIntegrity| [optional] Adds Package Integrity to every generated MSIX. Requires v5 of the template schema. |
|ValidInstallerExitCodes | [optional] 0 or more ValidInstallerExitCode elements. Requires v2 of the template schema. |
|ValidInstallerExitCodes:: ValidInstallerExitCode | [optional] Specify any installer exit codes that the tool might not be familiar with or require a reboot. Requires v2 of the template schema. |
|ValidInstallerExitCodes:: ValidInstallerExitCode:: Reboot | [optional] Specify whether an exit code should trigger a reboot during conversion. Requires v3 of the template schema. |
|ExclusionItems | [optional] 0 or more FileExclusion or RegistryExclusion elements. All FileExclusion elements must appear before any RegistryExclusion elements.|
|ExclusionItems::FileExclusion | [optional] A file to exclude for packaging.|
|ExclusionItems::FileExclusion::ExcludePath | Path to file to exclude for packaging.|
|ExclusionItems::RegistryExclusion | [optional] A registry key to exclude for packaging.|
|ExclusionItems::RegistryExclusion:: ExcludePath | Path to registry to exclude for packaging.|
|PrepareComputer::DisableDefragService | [optional] Disables Windows Defragmenter while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsSearchService | [optional] Disables Windows Search while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableSmsHostService | [optional] Disables SMS Host while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|PrepareComputer:: DisableWindowsUpdateService | [optional] Disables Windows Update while the app is being converted. If set to false, overrides ApplyAllPrepareComputerFixes.|
|SaveLocation | [optional] An element to specify the save location of the tool. If not specified, the package will be saved under the Desktop folder. |
|SaveLocation::PackagePath | [optional] The path to the file or folder where the resulting MSIX package is saved. |
|SaveLocation::TemplatePath | [optional] The path to the file or folder where the resulting command line template is saved. |
|Installer::Path | The path to the application installer.|
|Installer::Arguments | [optional] The arguments to pass to the installer.  The tool will automatically run MSI installers silently using argument  "/qn /norestart INSTALLSTARTMENUSHORTCUTS=1 DISABLEADVTSHORTCUTS=1". NOTE: You must pass the arguments to force your installer to run silently if you are using .exe installers.|
|Installer::InstallLocation | [optional] The full path to your application's root folder for the installed files if it were installed (e.g. "C:\Program Files (x86)\MyAppInstalllocation").|
|VirtualMachine | [optional] An element to specify that the conversion will be run on a local Virtual Machine.|
|VirtualMachine::Name | The name of the virtual machine to be used for the conversion environment.|
|VirtualMachine::Username | The user name for the virtual machine to be used for the conversion environment.|
|RemoteMachine | [optional] An element to specify that the conversion will be run on a remote machine. Requires v2 of the template schema. |
|RemoteMachine:: ComputerName | The name of the remote machine to be used for the conversion environment. Requires v2 of the template schema. |
|RemoteMachine:: Username | The user name for the remote machine to be used for the conversion environment. Requires v2 of the template schema. |
|RemoteMachine:: EnableAutoLogon | [optional] This will auto log you back in when performing a conversion that requires a restart on a remote machine so your conversion continues seamlessly. Requires V3 of the template schema. |
|PackageInformation::PackageName | The Package Name for your MSIX package.|
|PackageInformation::PackageDisplayName | The Package Display Name for your MSIX package.|
|PackageInformation::PublisherName | The Publisher for your MSIX package.|
|PackageInformation::PublisherDisplayName | The Publisher Display Name for your MSIX package.|
|PackageInformation::Version | The version number for your MSIX package.|
|PackageInformation::PackageDescription | [optional] The description for your MSIX package. Requires v4 of the template schema. |
|PackageInformation:: MainPackageNameForModificationPackage | [optional] The Package identity name of the main package name. This is used when creating a modification package that takes a dependency on a main (parent) application.|
|SigningInformation| [optional] An element to specify signing information for Device Guard signing. Requires v4 of the template schema. |
|SigningInformation:: DeviceGuardSigning | [optional] An element to specify Device Guard signing information. Requires v4 of the template schema. |
|DeviceGuardSigning:: TokenFile | The [Azure AD access token required](../package/signing-package-device-guard-signing.md#get-an-azure-ad-access-token) for Device Guard signing in JSON format. Requires v4 template schema. |
|DeviceGuardSigning:: TimestampUrl | [optional] Provides a timestamp at the time of signing with Device Guard to ensure that your application will install beyond the lifetime of the certificate. Requires v4 of the template schema. |
|Applications | [optional] 0 or more Application elements to configure the Application entries in your MSIX package.|
|Application::Id | The App ID for your MSIX application. This ID will be used for the Application entry detected that matches the specified ExecutableName. You can have multiple Application ID values for executables in the package.<br/><br/>This value is the unique identifier of the application within the package. This value is sometimes referred to as the package-relative app identifier (PRAID). The ID must be unique within the package (the same ID cannot be used more than once in the same package). However, the ID must not be unique globally. There may be another package on the system that uses the same ID.<br/><br/>This string contains alpha-numeric fields separated by periods. Each field must begin with an ASCII alphabetic character. You cannot use these as field values: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8", and "LPT9".|
|Application::DisplayName | The App Display Name for your MSIX package. This Display Name will be used for the application entry detected that matches the specified ExecutableName|
|Application::ExecutableName | The executable name for the MSIX application that will be added to the package manifest. The corresponding application entry will be ignored if no application with this name is detected.|
|Application::Description | [optional] The App Description for your MSIX application. If not used, the Application DisplayName will be used. This description will be used for the application entry detected that matches the specified ExecutableName|
|Capabilities | [optional] 0 or more Capability elements to add custom capabilities to your MSIX package. “runFullTrust” capability is added by default during conversion.|
|Capability::Name | The capability to add to your MSIX package. |

### Sample conversion template file

```xml
<MsixPackagingToolTemplate
    xmlns="http://schemas.microsoft.com/appx/msixpackagingtool/template/2018"
    xmlns:V2="http://schemas.microsoft.com/msix/msixpackagingtool/template/1904"
    xmlns:V3="http://schemas.microsoft.com/msix/msixpackagingtool/template/1907"
    xmlns:V4="http://schemas.microsoft.com/msix/msixpackagingtool/template/1910"
    xmlns:V5="http://schemas.microsoft.com/msix/msixpackagingtool/template/2001">
<!--Note: You only need to include xmlns:v2 - xmlns:v5 if you are using one of the features that use those schemas -->

    <Settings
        AllowTelemetry="true"
        ApplyAllPrepareComputerFixes="true"
        GenerateCommandLineFile="true"
        AllowPromptForPassword="false" 
	    EnforceMicrosoftStoreVersioningRequirements="false"
        v2:ServerPortNumber="1599"
        v5:AddPackageIntegrity="true">    

	<!--Note: Exclusion items are optional and if declared take precedence over the default tool exclusion items
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
	    <FileExclusion ExcludePath="[{Windows}]\Installer" />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\$Recycle.Bin " />
            <FileExclusion ExcludePath="[{AppVPackageDrive}]\System Volume Information" />
	    <FileExclusion ExcludePath="[{AppVPackageDrive}]\Config.Msi" />
            <FileExclusion ExcludePath="[{AppData}]\Microsoft\AppV" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Microsoft Antimalware" />
            <FileExclusion ExcludePath="[{Common AppData}]\Microsoft\Windows Defender" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Microsoft Security Client" />
            <FileExclusion ExcludePath="[{ProgramFiles}]\Windows Defender" />
	    <FileExclusion ExcludePath="[{ProgramFiles}]\WindowsApps" />
            <FileExclusion ExcludePath="[{Local AppData}]\Temp" />
	    <FileExclusion ExcludePath="[{Local AppData}]\Microsoft\Windows" />
	    <FileExclusion ExcludePath="[{Local AppData}]\Packages" />

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
	-->
    
    <!--Note: Specifying an installer exit code will allow you to automatically trigger a reboot during your conversion
      <v2:ValidInstallerExitCodes>
        <V2:ValidInstallerExitCode ExitCode="3010" V3:Reboot="true"/>
        <V2:ValidInstallerExitCode ExitCode="1641"/>
      </v2:ValidInstallerExitCodes>
    -->
	    
    </Settings>

    <!--Note: this section takes precedence over the Settings::ApplyAllPrepareComputerFixes attribute and is optional
    <PrepareComputer
        DisableDefragService="true"
        DisableWindowsSearchService="true"
        DisableSmsHostService="true"
        DisableWindowsUpdateService="true"/>
    -->

    <SaveLocation
        PackagePath="C:\users\user\Desktop\MyPackage.msix" 
        TemplatePath="C:\users\user\Desktop\MyTemplate.xml" />

    <Installer
        Path="C:\MyAppInstaller.msi"
        InstallLocation="C:\Program Files\MyAppInstallLocation" />
	
    <!--NOTE: This section specifies that the conversion will be run on a local Virtual Machine. This is optional if you want to change your conversion environment from the default local machine.
    <VirtualMachine Name="vmname" Username="vmusername"/>
    -->

    <!--NOTE: This section specifies that the conversion will be run on a remote machine.This is optional if you want to change your conversion environment from the default local machine.
    <v2:RemoteMachine ComputerName="vmname" Username="vmusername" v3:EnableAutoLogon="true"/>
    -->

    <PackageInformation
        PackageName="MyAppPackageName"
        PackageDisplayName="MyApp Display Name"
        PublisherName="CN=MyPublisher"
        PublisherDisplayName="MyPublisher Display Name"
        Version="1.1.0.0"
        MainPackageNameForModificationPackage="MainPackageIdentityName">

    <!--Note: This is optional, if you want to sign your package with Device Guard signing
        <v4:SigningInformation>
            <v4:DeviceGuardSigning
                Tokenfile="tokenfile.json"
                TimestampUrl="https://mytimestamp.com"/>
        </v4:SigningInformation>
    -->
        
	<!--NOTE: This ID will be used if the Application entry detected matches the specified ExecutableName
        <Applications>
            <Application
                Id="MyApp1"
                Description="MyApp"
                DisplayName="My App"
                ExecutableName="MyApp.exe"/>
        </Applications>
	-->

	<!--NOTE: This is optional as “runFullTrust” capability is added by default during conversion
        <Capabilities>
            <Capability Name="runFullTrust" />
        </Capabilities>
	-->
	    
    </PackageInformation>
</MsixPackagingToolTemplate>
```
