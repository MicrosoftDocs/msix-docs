---
description: This article describes how to create and install an unsigned MSIX package
title: Create an unsigned MSIX package for testing
ms.date: 01/11/2022
ms.topic: article
keywords: windows 10, uwp, msix
---

# Create an unsigned MSIX package for testing

Starting in Windows 11, we now allow developers to install their apps via Powershell without needing to sign their package. This feature is intended for testing purposes and should not be used to distribute the app widely. This is useful for developers making it easier for them to easily and quickly test their apps.

## How to create an unsigned package

Unsigned packages must include a special OID (organization ID) value in their [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the manifest file or they won’t be allowed to register. An unsigned package will never have the same identity as a package that is signed. This prevents unsigned packages from conflicting with or spoofing the identity of a signed package.

Example: 

```xml
...
  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />
...
```

## Installing an unsigned package

Developers must run PowerShell as admin when installing an unsigned package, and pass the **-AllowUnsigned** flag when running the [Add-AppxPackage](/powershell/module/appx/add-appxpackage&preserve-view=true) command. A non admin can only install unsigned packages if it contains non executable files. This is useful in scenarios where the user only needs to load images, assets and content or script files. 

Example:

```powershell
    Add-AppPackage -Path ".\MyEmployees.appx" -AllowUnsigned
```

When the app is ready to be distributed, developers should ensure the package is signed. They must remove the special OID and ensure that the publisher name is the same as the certificate subject name.
