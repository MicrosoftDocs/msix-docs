---
title: Update your existing MSIX to support MSIX Core 
description: This article provides an overview of MSIX Core, which brings MSIX support to Windows 7 SP1, Windows 8.1, currently supported Windows Server (with desktop experience), and Windows 10 versions prior to 1709 (Fall Anniversary Update).
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Update your existing MSIX package to support MSIX Core

Before you can deploy your MSIX package with [MSIX Core](msixcore.md), you must first update your MSIX package manifest.

Apps packaged as MSIX must be compatible with the operating system in which they are being deployed. The MSIX package manifest must contain a proper **TargetDeviceFamily** with the name **MSIXCore.Desktop** and a **MinVersion** matching the operating system build number. Make sure to also include the relevant Windows 10, version 1709 and later entry as well so the app will deploy properly on operating systems that natively support MSIX.

The following example specifies Windows 7 SP1 as a minimum version:

```xml
  <Dependencies>
    <TargetDeviceFamily Name="MSIXCore.Desktop" MinVersion="6.1.7601.0" MaxVersionTested="10.0.10240.0" />
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested="10.0.18362.0" />
  </Dependencies>
```

All **MSIXCore.Desktop** apps will deploy to Windows Server with Desktop Experience based operating systems with the same build number. If the app is intended only for a server operating system, specify a **TargetDeviceFamily** with the name **MSIXCore.Server**. Deployment to Windows Server Core is not supported.

## Update Manifest using the MSIX Packaging Tool Package Editor
If you have an MSIX package, you can use the MSIX Package Tool to update your existing package to support MSIX Core without repackaging. You can do it two ways through the Package Editor:

1. Open **MSIX Packaging Tool** app
2. Select **Package editor** 
3. Click on **Browse...** to locate your package
4. Click **Open package**

### [Option 1] Use the checkbox and dropdown to add support
5. Under **MSIX Core Support**, select the checkbox to **Add support for MSIX Core to this package**
6. Select the Windows version you would like supported for this package


### [Option 2] Manually add in the manifest file
5. Under **Manifest file**, click **Open File**
6. You are viewing the package's manifest. Under **Dependency** add MSIX Core as a Target Device Family (see above)
7. Save and Close the manifest 

8. Re-sign the package 
9. Click **Save** and select whether you would like your package to increment 

## Add MSIX Core support using the MSIX Packaging Tool during conversion
Starting in version 1.2020.402.0, you can add MSIX Core support to each MSIX package that you generate with the MSIX Packaging Tool. 

### Add MSIX Core support to all MSIX Packages
1. Open **MSIX Packaging Tool** app
2. Select the gear in the top right to access the **settings**
3. Under **Tool Defaults** select the checkbox to **Add support for MSIX Core when generating a package.**
4. Select the Windows version you would like support for by default
5. Save settings

### Add MSIX Core support to a single package during workflow
During conversion of an existing installer, you can choose to add MSIX Core support to the package you are generating, if you don't have it specified as a default setting. You can also overwrite the default setting that you have specified in your settings. 

1. On the Package Information step of conversion, select the checkbox to **Add support for MSIX Core to this package**
2. Select the Windows version you would like supported for this package
3. Continue with your conversion process

## Windows versions supported by MSIX Core

| Name | Version |
|------|---------|
| Windows 7, SP 1| 6.1.7601.0|
| Windows 8.1 (Update 1) |6.3.9600.0|
| Windows 10 2015 LTSB (1507)|10.0.10240.0|
| Windows 10 2016 LTSB (1607)|10.0.14393.0|
| Windows Server 2008 R2| 6.1.7601.0|
| Windows Server 2012| 6.2.9200.0|
| Windows Server 2012 R2| 6.3.9600.0|
| Windows Server 2016 | 10.0.14393.0|
