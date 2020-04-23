---
title: MSIX Bulk conversion scripts
description: This article provides details about the bulk conversion scripts in the MSIX Toolkit.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX Bulk conversion scripts

The [Bulk conversion scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) in the MSIX Toolkit can be used to automate the conversion of Windows apps to the MSIX package format. The list of apps and their details are provided in the **entry.ps1** script.

## Syntax

```powershell
entry.ps1
```

## Description

This is a set of PowerShell scripts that provides the ability to bulk package applications into the MSIX package format. These scripts will connect to a local virtual machine or remote machine which will be used to package each application.

Apps being packaged into the MSIX application format will be converted in the order they were entered in the **entry.ps1** script. Remote machines listed in the **entry.ps1** script will be used to package the applications into the MSIX format will be singularly used. Virtual machines can be used multiple times to package different applications into the MSIX application format.

Before running the script, you must first add the apps you want to convert to the `conversionsParameters` variable in the script. Multiple apps can be added to the variable. The script leverages the app and remote/virtual machines to create a XML file formatted to meet the requirements of the [MSIX Packaging Tool](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe). After creating the XML file, the **run_job.ps1** script is executed in a new PowerShell process which executes MsixPackagingTool.exe on the target device to convert the app and place it in the **.\Out** folder located in the script execution folder.

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

```

```powershell
$virtualMachines = @(
    @{
        Name = "MSIX Packaging Tool Environment";   # Name of the virtual machine as listed in the Hyper-V Management console
        Credential = $credential                    # Credentials used to connect/login to the virtual machine.
    }
)
```

The specified virtual machine will be used to package apps into the MSIX format. This virtual machine will be connected to using the credentials entered when prompted (prompt appears directly after script **entry.ps1** execution).

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

### conversionsParameters

The `conversionsParameters` parameter is an array that contains information about the apps you want to convert to MSIX format. Each app in the array will be parsed individually and run through the MSIX package conversion on either a remote machine or virtual machine. The apps will be converted in the order that they appear in the script. If the conversion to MSIX format fails, the script will not re-attempt to convert the application on a different remote machine or virtual machine.

* **Type:** Array
* **Required:** Yes

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

The app information provided in the `conversionsParameters` variable will be used to generate an XML file with all of the required application details. After creating the XML file, the script will then pass the XML file to the [MSIX Packaging Tool](..\packaging-tool\mpt-overview.md) (MsixPackagingTool.exe) to be packaged.
