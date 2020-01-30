---
title: MSIX Modify Package Publisher
description: This article provides details about the available MSIX package publisher Script.
ms.date: 1/23/2020
ms.topic: article
keywords: windows 10, msix, msix toolkit
ms.localizationpriority: medium
ms.custom: 19H1
---

# MSIX Connect Scripts
## SYNOPSIS
This script will update the publisher in the app manifest before re-signing the package based on a new certificate. This script is currently limited to MSIX applications, and not MSIXBundles. 

## SYNTAX
```powershell
.\modify-package-publisher.ps1 -directory <String> -redist <String> -certPath <String> [[-pfxPath] <String>] [[-Password] <String>] [[-forceContinue]<Switch>]
```

## DESCRIPTION

## Examples
### Example 1: Updates the publisher based on the certificate
```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer"
```

This will recursively search the contents of *C:\MSIX* locating all *.MSIX applications, then update the MSIX application publisher to match the publisher of the certificate located at *C:\cert\mycert.cer*.

### Example 2: Updates the publisher and signs the MSIX application.
```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx"
```

This will recursively search the contents of *C:\MSIX* locating all *.MSIX applications, then updates the MSIX application publisher to match the publisher of the certificate located at *C:\cert\mycert.cer*. Then re-sign's the identified MSIX applications using the certificate located at *C:\cert\CertKey.pfx*.

### Example 3:  Updates the publisher and signs the MSIX application with a password protected PFX certificate.
```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -password "aaabbbccc"
```

This will recursively search the contents of *C:\MSIX* locating all *.MSIX applications, then updates the MSIX application publisher to match the publisher of the certificate located at *C:\cert\mycert.cer*. Then re-sign's the identified MSIX applications using the certificate located at *C:\cert\CertKey.pfx* using the password *aaabbbccc* to unlock the password protected certificate.

### Example 4: Updates the publisher, signs the MSIX application and force continues to next MSIX application.
```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -forceContinue -pfxPath "C:\cert\CertKey.pfx"
```

This will recursively search the contents of *C:\MSIX* locating all *.MSIX applications, then updates the MSIX application publisher to match the publisher of the certificate located at *C:\cert\mycert.cer*. Then re-sign's the identified MSIX applications using the certificate located at *C:\cert\CertKey.pfx*. If any errors occur while processing a single MSIX application, the script will continue to update the publisher and re-sign the identified MSIX applications.

## PARAMETERS
### -Directory
Provides the root directory which contains MSIX applications. This directory will be recursively searched for all *.msix applications.

```yaml
Type: String
Required: True

Position: Named
Default Value: None
```

### -CertPath
Provides the full path to the certificate file (*.cer) used to identify the new or updated application publisher information.

```YAML
Type: String
Required: True

Position: Named
Default Value: None
```

### -Redist
The path to the redistributable file retrieved from within the [MSIX Toolkit](http://aka.ms/msixtoolkit). This file is used to re-package the application into the MSIX package format. Must point to either the 32-bit or 64-bit architecture redistributable.

```YAML
Type: String
Required: True

Position: Named
Default Value: None
```

### -PfxPath
The path to the code signing certificate (*.pfx) which will be used to sign the MSIX application after updating the application publisher.

```YAML
Type: String
Required: False

Position: Named
Default Value: None
```

### -Password
The password required by the code signing certificate (*.pfx).

```YAML
Type: String
Required: False

Position: Named
Default Value: None
```

### -ForceContinue
If specified, the script will ignore errors and attempt to update the publisher information of all application.

```YAML
Type: Switch
Required: False

Position: Named
Default Value: False
```

## INPUTS
## OUTPUTS
## NOTES
