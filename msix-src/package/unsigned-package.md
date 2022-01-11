---
description: This article describes how to create and install an unsigned MSIX package
title: Create an unsigned MSIX package 
ms.date: 01/11/2022
ms.topic: article
keywords: windows 10, uwp, msix
---

# Create an unsigned MSIX package

Starting in Windows 11, we now allow developers to install their apps via Powershell without needing to sign their package. This is useful for developers making it easier for them to easily and quickly test their apps.

## How to create an unsigned package

Unsigned packages must include a special OID (organization ID) value in their [Identity](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element or they won’t be allowed to register. An unsigned package will never have the same identity as a package that is signed. This prevents unsigned packages from conflicting with or spoofing the identity of a signed package.

```xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

## Installing an unsigned package

Users should run as admin when installing an unsigned package, and pass the **-AllowUnsigned** flag when running the [Add-AppxPackage](https://docs.microsoft.com/en-us/powershell/module/appx/add-appxpackage?view=windowsserver2022-ps) command. A non admin can only install unsigned packages if it contains non executable files. This is useful in scenarios where the user only needs to load images, assets and content or script files. 

