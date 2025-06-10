---
title: Modify package publisher script
description: This article provides details about the modify package publisher script in the MSIX Toolit.
ms.date: 1/23/2020
ms.topic: how-to
keywords: windows 10, msix, msix toolkit
ms.custom: 19H1
---

# Modify package publisher script

The [Modify package publisher script](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts/ModifyPackagePublisher) in the MSIX Toolkit can be used to update the publisher in the manifest before re-signing the package based on a new certificate. This script is currently limited to MSIX apps, and not MSIX bundles.

## Syntax

```powershell
.\modify-package-publisher.ps1 -directory <String> -redist <String> -certPath <String> [[-pfxPath] <String>] [[-Password] <String>] [[-forceContinue]<Switch>]
```

## Examples

### Update the publisher based on the certificate

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer"
```

This command recursively searches the contents of C:\MSIX for all MSIX packages and updates the MSIX app publisher to match the publisher of the certificate located at C:\cert\mycert.cer. Signing an MSIX package format application with a SHA1 certificate is unsupported.

### Update the publisher and sign the MSIX app

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx"
```

This command recursively searches the contents of C:\MSIX for all MSIX packages and updates the MSIX app publisher to match the publisher of the certificate located at C:\cert\mycert.cer. Then, the command re-signs the identified MSIX packages using the certificate located at C:\cert\CertKey.pfx. Signing the MSIX package format application with a SHA1 certificate is unsupported.

### Update the publisher and sign the MSIX app with a password-protected PFX certificate

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -password "aaabbbccc"
```

This command recursively searches the contents of C:\MSIX for all MSIX packages and updates the MSIX app publisher to match the publisher of the certificate located at C:\cert\mycert.cer. Then, the command re-signs the identified MSIX packages using the certificate located at C:\cert\CertKey.pfx using the password *aaabbbccc* to unlock the password protected certificate. Signing the MSIX package format application with a SHA1 certificate is unsupported.

### Update the publisher, sign the MSIX app, and force continue to next MSIX app

```powershell
PS C:\> .\modify-package-publisher.ps1 -directory "C:\MSIX" -redist "C:\MSIX-Toolkit\Redist" -certPath "C:\cert\mycert.cer" -pfxPath "C:\cert\CertKey.pfx" -forceContinue -pfxPath "C:\cert\CertKey.pfx"
```

This command recursively searches the contents of C:\MSIX for all MSIX packages and updates the MSIX app publisher to match the publisher of the certificate located at C:\cert\mycert.cer. Then, the command re-signs the identified MSIX packages using the certificate located at C:\cert\CertKey.pfx. If any errors occur while processing an MSIX package, the script will continue to update the publisher and re-sign the identified MSIX packages. Signing the MSIX package format application with a SHA1 certificate is unsupported.

## Parameters

### -directory

Provides the root directory which contains MSIX applications. This directory is recursively searched for all MSIX packages.

* **Type:** String
* **Required:** Yes
* **Position:** Named
* **Default value:** None

### -certPath

Provides the full path to the certificate file (*.cer) used to identify the new or updated app publisher information.

* **Type:** String
* **Required:** Yes
* **Position:** Named
* **Default value:** None

### -redist

The path to the redistributable file retrieved from within the [MSIX Toolkit](https://aka.ms/msixtoolkit). This file is used to re-package the app into the MSIX package format. Must point to either the 32-bit or 64-bit architecture redistributable.

* **Type:** String
* **Required:** Yes
* **Position:** Named
* **Default value:** None

### -pfxPath

The path to the code signing certificate (*.pfx) which will be used to sign the MSIX package after updating the app publisher.

* **Type:** String
* **Required:** No
* **Position:** Named
* **Default value:** None

### -password

The password required by the code signing certificate (*.pfx).

* **Type:** String
* **Required:** No
* **Position:** Named
* **Default value:** None

### -forceContinue

If specified, the script will ignore errors and attempt to update the publisher information of all apps.

* **Type:** String
* **Required:** No
* **Position:** Named
* **Default value:** None
