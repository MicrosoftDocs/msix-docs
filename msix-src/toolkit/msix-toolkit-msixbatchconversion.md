---
title: MSIX Bulk conversion scripts
description: This article provides details about the bulk conversion scripts in the MSIX Toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msixtoolkit, msix toolkit, toolkit, batch, conversion, bulk, bulk conversion
ms.custom: 19H1
---

# MSIX Bulk conversion scripts

The [Bulk conversion scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BulkConversion) in the MSIX Toolkit can be used to automate the conversion of Windows apps to the MSIX package format. The list of apps and their details are provided in the **entry.ps1** script.

## Prepare machines for conversion
Prior to running the MSIX Toolkit's Bulk Conversion script, to automate the conversion of your application to the MSIX packaging format the devices that you will be using (virtual, or remote) must be configured to allow remote communication, and have the MSIX Packaging Tool installed.

| Term            | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| Host Machine    | This is the device executing the Bulk Conversion scripts.          |
| Virtual Machine | This is a device existing in Hyper-V, hosted on the Host Machine.  |
| Remote Machine  | This is a physical or virtual machine accessible over the network. |

### Host Machine
The Host Machine must meet the following requirements:
* [MSIX Packaging Tool](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) must be installed.
* If Virtual Machines are being used, Hyper-V must be installed.
* If Remote Machines are being used:
    * Device exists in the same Domain as the Remote Machine(s):
        * Enable PowerShell Remoting 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            ```
    * Device exists in a Workgroup or to an alternate Domain as the Remote Machine(s):
        * Enable PowerShell remoting 
        * WinRM Trusted Host must contain the device name or IP address of the Remote Machine 
            ```PowerShell
            # Enables PowerShell Remoting
            Enable-PSRemoting -force
            Set-Item WSMan:\localhost\Client\TrustedHosts -Value <RemoteMachineName>,[<RemoteMachineName>,...]
            ```

### Remote Machine
The Remote Machine must meet the following requirements:
* [MSIX Packaging Tool](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) must be installed.
* If the device exists within the same domain as the Host Machine:
    * Enable PowerShell Remoting 
    * WinRM must be enabled 
    * Allow ICMPv4 through client firewall
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        ```

* If the device exists within a workgroup or an alternate domain as the Host Machine:
    * Enable PowerShell Remoting 
    * WinRM Trusted Host must contain the device name or the IP address of the Host Machine
    * Allow ICMPv4 through client firewall
        ```PowerShell
        # Enables PowerShell Remoting
        Enable-PSRemoting -force
        New-NetFirewallRule -DisplayName “ICMPv4” -Direction Inbound -Action Allow -Protocol icmpv4 -Enabled True
        Set-Item WSMan:\localhost\Client\TrustedHosts -Value <HostMachineName>
        
        ```

### Virtual Machine
It is recommended that the Hyper-V Quick Create "MSIX Packaging Tools Environment" image be used, as it is pre-configured to meet all requirements. The virtual machine must be hosted on the Host Machine and running within Microsoft Hyper-V.

The Virtual Machine must meet the following requirements:
* [MSIX Packaging Tool](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab) must be installed.

## Syntax

```powershell
entry.ps1
```

## Description

This is a set of PowerShell scripts that provides the ability to bulk package applications into the MSIX package format. These scripts will connect to a local virtual machine or remote machine which will be used to package each application.

Apps being packaged into the MSIX application format will be converted in the order they were entered in the **entry.ps1** script. Remote machines listed in the **entry.ps1** script will be used to package the applications into the MSIX format will be singularly used. Virtual machines can be used multiple times to package different applications into the MSIX application format.

Before running the script, you must first add the apps you want to convert to the `conversionsParameters` variable in the script. Multiple apps can be added to the variable. The script leverages the app and remote/virtual machines to create a XML file formatted to meet the requirements of the [MSIX Packaging Tool](../packaging-tool/tool-overview.md) (MsixPackagingTool.exe). After creating the XML file, the **run_job.ps1** script is executed in a new PowerShell process which executes MsixPackagingTool.exe on the target device to convert the app and place it in the **.\Out** folder located in the script execution folder.

## Example

```powershell
PS C:\> entry.ps1
```

Ths example executes the **entry.ps1** script. This script converts the apps specified in the `conversionsParameters` variable into MSIX packages. The apps are converted using the virtual machines or remote machines indicated in the *virtualMachines* and *remoteMachines* variables.

## Parameters

### virtualMachines

The `virtualMachines` parameter is an array that contains the name and credentials of the virtual machines to connect to and access when packaging an app into the MSIX format.

* **Type:** Array
* **Required:** No

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

The specified virtual machine will be used to package apps into the MSIX format. This virtual machine will be connected to using the credentials entered when prompted (prompt appears directly after script **entry.ps1** execution). Prior to packaging an application to the MSIX packaging format, the script will create a snapshot of the Hyper-V VM, and then restored to this snapshot after application has been packaged.

### remoteMachines

The `remoteMachines` parameter is an array that contains the name and credentials of the remote machines to connect to and access when packaging an app into the MSIX format. The remote machines specified will be single-use devices used to package a single application.

Remote machines must be accessible and discoverable on the network.

* **Type:** Array
* **Required:** No

```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

The specified remote machine will be used to package a single app into the MSIX format. This remote machine will be connected to using the credentials entered when prompted (prompt appears directly after script **entry.ps1** execution).

Ensure that the fully qualified domain name or externally facing alias of the device is resolvable prior to execution of the **entry.ps1** script.

### signingCertificate
The `signingCertificate` parameter is an array that contains information related to the code signing certificate that will be used to sign the MSIX packaged application. This certificate must have an encryption level of at minimum SHA256.

* **Type:** Array
* **Required:** No

```powershell
$SigningCertificate = @{
    Password = "Password"; 
    Path = "C:\Temp\ContosoLab.pfx"
}
```
### conversionsParameters

The `conversionsParameters` parameter is an array that contains information about the apps you want to convert to MSIX format. Each app in the array will be parsed individually and run through the MSIX package conversion on either a remote machine or virtual machine. The apps will be converted in the order that they appear in the script. If the conversion to MSIX format fails, the script will not re-attempt to convert the application on a different remote machine or virtual machine.

* **Type:** Array
* **Required:** Yes

```powershell
$conversionsParameters = @(
    ## Use for MSI applications:
    @{
        InstallerPath = "C:\Path\To\YourInstaller.msi";    # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0"                         # MSIX Application version (must contain 4 octets).
    },
    ## Use for EXE or other applications:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement"   # Arguements required by the installer to provide a silent installation of the application.
    },
    ## Creating the Packaged app and Template file in a specific folder path:
    @{
        InstallerPath = "Path\To\YourInstaller.exe";       # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                           # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                   # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                  # Certificate Publisher information - must match signing certificate
        PublisherDisplayName = "YourCompany";              # Application Publisher name
        PackageVersion = "1.0.0.0";                        # MSIX Application version (must contain 4 octets).
        InstallerArguments = "/SilentInstallerArguement";  # Arguements required by the installer to provide a silent installation of the application.
        SavePackagePath = "Custom\folder\Path";            # Specifies a custom folder path where the MSIX app will be created.
        SaveTemplatePath = "Custom\folder\Path"            # Specifies a custom folder path where the MSIX Template XML will be created.
    }
)
```

The app information provided in the `conversionsParameters` variable will be used to generate an XML file with all of the required application details. After creating the XML file, the script will then pass the XML file to the [MSIX Packaging Tool](../packaging-tool/tool-overview.md) (MsixPackagingTool.exe) to be packaged.

## Logging

The script will generate a log file which outlines what has transpired throughout the script execution. The log file will provide details related to the packaging of applications to the MSIX packaging format, and information related to script progression. The logs can be read from any text utility, but have been configured for reading using the Trace32 log reader. Errors in the script execution will be highlighted as Red, and Warnings as yellow. For more information on the Trace 32 log reader, please visit [CMTrace](/mem/configmgr/core/support/cmtrace).

The log file is created within the script's directory `.\logs\BulkConversion.log`.
