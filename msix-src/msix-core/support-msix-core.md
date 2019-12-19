---
title: Update your existing MSIX to support MSIX Core 
description: This article provides an overview of MSIX Core, which brings MSIX support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update).
ms.date: 04/12/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Update your existing MSIX to support MSIX Core 
In order to use your existing MSIX packages on Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update), you will need to update the MSIX package manifest. Apps packaged as MSIX must be compatible with the operating system in which they are being deployed.  The MSIX package manifest must contain a proper **TargetDeviceFamily** with the name MSIXCore.Desktop and a MinVersion matching the operating system build number.  Make sure to also include the relevant Windows 10 1709 and later entry as well so the app will deploy properly on operating systems that natively support MSIX.

Example for Windows 7 SP1 as a minimum version:

```
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

All MSIXCore.Desktop apps will deploy to Windows Server with Desktop experience based operating systems with the same build number.  If the app is intended only for a server operating system then use the TargetDeviceFamily of MSIXCore.Server.  Deployment to Windows Server Core is not supported.

## Update Manifest using the MSIX Packaging Tool 
If you have an MSIX package, you can use the MSIX Package Tool to update your package to support MSIX Core. Here are the steps: 
1. Open **MSIX Packaging Tool** app
2. Select **Package editor** 
3. Click on **Browse...** to locate your package
4. Click **Open package**
5. Under **Manifest file**, click **Open File**
6. You are viewing the package's manifest. Under **Dependency** add MSIX Core as a Target Device Family (see above)
7. Save and Close the manifest 
8. Resign the package 
9. Click **Save** and select whether you would like your package to increment 

## Supported Versions of Windows with MSIX Core 

| Name | Version |
|------|---------|
| Windows 7, service pack 1| 6.1.7601.0|
| Windows 8.1 (Update 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
