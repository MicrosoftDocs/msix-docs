---
title: MSIX Connect Script
description: This article provides details about the available MSIX Connect Script.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX Connect Scripts
## Synopsis
Converts a Microsoft Endpoint Configuration Manager application to MSIX application package format.


## Syntax
```powershell
New-MSIXConnectMakeMSIX [[-SiteCode] <String>] [[-SiteServerName] <String>] [[-ApplicationName] <String>]
```

## Description
The **New-MSIXConnectMakeMSIX** PowerShell cmdlet retrieves application information from the Microsoft Endpoint Configuration Manager application. A Microsoft Endpoint Configuration Manager application contains metadata about the application, as well as one or more deployment types. Each deployment type includes installation files and information required to install the software onto a device. This information is collected and passed through to the [BatchConversion](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/BatchConversion) script and automatically converted to the MSIX application package format. All Deployment Types are converted for each supplied application.

## Examples
### Example 1: Convert an application
```powershell
PS C:\> . .\Get-MSIXConnectLib.ps1
PS C:\> New-MSIXConnectMakeMSIX -SiteCode "CM1" -SiteServerName "ServerName" -ApplicationName "Application1"
```

The first command retrieves the PowerShell library which makes the **New-MSIXConnectMakeMSIX** PowerShell cmdlet available for use.

The second command passes through the System Center Configuration Manager environment information, as well as the name of the application. The application name is the same as is listed within the System Center Configuration Manager environment.

### Example 2: Convert multiple applications
```powershell
PS C:\> . .\Get-MSIXConnectLib.ps1
PS C:\> @("Application1", "Application2", "Application3") | foreach{New-MSIXConnectMakeMSIX -SiteCode "CM1" -SiteServerName "ServerName" -ApplicationName $_}
```
The first command retrieves the PowerShell library which makes the **New-MSIXConnectMakeMSIX** PowerShell cmdlet available for use.

The second command creates an array of application names, then passes each one individually into the **New-MSIXConnectMakeMSIX** PowerShell cmdlet as well information pertaining to the System Center Configuration Manager environment.

## Parameters
### -SiteCode
Specifies the Microsoft Endpoint Configuration Manager Site Code assigned to Primary Site where the application resides.

```YAML
Type: String
Aliases: N/A

Required: True
Position: 0
Default Value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SiteServerName
Specifies the Microsoft Endpoint Configuration Manager Site server name.

```YAML
Type: String
Alias: N/A

Required: True
Position: 1
Default Value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationName
Specifies the name of the application in Microsoft Endpoint Configuration Manager which is to be converted.

```YAML
Type: String
Alias: N/A

Required: True
Position: 2
Default Value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## Inputs
## Outputs
## Notes
