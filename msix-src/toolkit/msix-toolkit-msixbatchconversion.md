---
title: MSIX Batch Conversion Script
description: This article provides details about the available MSIX Batch Conversion Script.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX Batch Conversion
## SYNOPSIS
This is a collection of scripts which can be used to to atomate the conversion of applications to the MSIX application package format. List of application and their details are provided within the **entry.ps1** script.

## SYNTAX
```powershell
entry.ps1
```

## DESCRIPTION
A set of PowerShell scripts which provides the ability to bulk package applications into the MSIX application package format. These scripts will connect to a local virtual machine or remote machine which will be used to package each application.

Applications being packaged into the MSIX application format, will be converted in the order they were entered in the **entry.ps1** script. Remote machines listed in the **entry.ps1** script will be used to package the applications into the MSIX application format will be singularilly used. While virtual machines could be used multiple times to package different applications into the MSIX application format.

The application(s) being converted into the MSIX application package format must first be added to the **entry.ps1** script, providing the required details to the *conversionsParameters* variable. Multiple applications can be added to the *conversionsParameters* variable array. The script will then leverage the application, remote/virtual machines to create a XML file formatted to meet the requirements of the [MsixPackagingTool.exe]. After creating the XML file, the **run_job.ps1** script is executed in a new PowerShell process which will execute the *MsixPackagingTool.exe* on the target device converting the application and storing it into the *.\Out* folder located in the script execution folder.

## Examples
### Example 1: Convert multiple applications
```powershell
PS C:\> entry.ps1
```

Ths will execute the **entry.ps1** script triggering the applications outlined within the *ConversionParameters* variable array to be converted into MSIX package formatted applications. The applications will be converted using the virtual machines, or remote machines outlined within the *virtualMachines* and *remoteMachines* variable arrays.

## PARAMETERS
### Virtual Machines
The virtual machine name and supplied credentails stored within the variable array will be used to connect and access the virtual machine when packaging an application into the MSIX application format.

```YAML
Type: Array
Required: Optional
```
```powershell
$virtualMachines = @(
    @{ 
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

The above listed virtual machine will be used to package applications into the MSIX application format. This virtual machine will be connected to using the credentials entered when prompted (prompt appears directly after script **entry.ps1** execution).

### Remote Machines
The remote machine name and supplied credentials stored within the variable array will be used to connect and access the remote machine when packaging an application into the MSIX application format. The remote machines listed, will be single use devices used to package a single application.

Remote machines must be accessible and discoverable on the network.

```yaml
Type: Array
Required: Optional
```
```powershell
$remoteMachines = @(
    @{
        ComputerName = "Computer.Domain.com";   # The fully qualified name of the remote machine.
        Credential = $credential }              # Credentials used to connect/login to the remote machine.
)
```

The above listed remote machine will be used to package a single application into the MSIX application format. This remote machine will be connected to using the credentials entered when prompted (prompt appears directly after script **entry.ps1** execution).

Ensure that the fully qualified domain name or externally facing alias of the device is resolvable prior to execution of the **entry.ps1** script.

### Applications
The applications listed within the *ConversionsParameters* variable array will be parsed through individually. Each application will be run through the MSIX application package conversion on either a remote machine, or virtual machine. These applications will be converted in the order that they appear. If the conversion to MSIX packaging format fails, the script will not re-attempt to convert the application on a different remote machine or virtual machine.

```YAML
Type: Array
Required: True
```
```powershell
$conversionsParameters = @(
    @{
        InstallerPath = "Path\To\Your\Installer\YourInstaller.msi"; # Full path to the installation media (local or remote paths).
        PackageName = "YourApp";                                    # Application Display Name - name visible in the start menu.
        PackageDisplayName = "Your App";                            # Application Name - Can not contain special characters.
        PublisherName = "CN=YourCompany";                           # Certificate Publisher information
        PublisherDisplayName = "YourCompany";                       # Application Publisher name
        PackageVersion = "1.0.0.0"                                  # MSIX Application version (must contain 4 octets).
    }
)
```
The application information provided inside of the *conversionsParameters* variable will be used to generate an XML file with all of the required application details. After creating the XML file, the script will then pass the XML file to the [MsixPackagingTool.exe] to be packaged. 


## INPUTS
## OUTPUTS
## NOTES