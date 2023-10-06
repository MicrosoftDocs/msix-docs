---
title: Create an unsigned MSIX package
description: This topic describes how to create and install an unsigned MSIX package
ms.date: 05/26/2023
ms.topic: article
keywords: windows 11, uwp, desktop, msix
---

# Create an unsigned MSIX package

As of Windows 11, you can install your app via PowerShell without needing to sign your package. This feature is intended to make it easier for you to quickly test your app. Don't use this feature to distribute your app widely.

## Create an unsigned package

An unsigned package must include a special OID (organization ID) value in its [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) element in the manifest file, otherwise it won't be allowed to register. An unsigned package will never have the same identity as a package that's signed. That prevents unsigned packages from conflicting with, or spoofing the identity of, a signed package.

Here's an example.

```xml
...
<Identity Name="NumberGuesserManifest"
  Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
  Version="1.0.0.0" />
...
```

## Install an unsigned package

> [!IMPORTANT]
> In most scenarios, you'll need to run PowerShell as administrator. See the details below.

* To install an unsigned package, pass the `-AllowUnsigned` flag to the [Add-AppxPackage](/powershell/module/appx/add-appxpackage) command.
* In most scenarios, the unsigned package will contain executable content; so you'll need to run PowerShell as administrator. That's because an unsigned package containing executable content must be installed for all users. Since that can affect more than just the current user, it requires administrator privilege.
* If the unsigned package contains only non-executable content (for example, when you need to load only images, assets and other content, or script files) then administrator privilege is *not* needed, and a non-admin can install the package.

Here's an example of the syntax.

```powershell
Add-AppPackage -Path ".\MyEmployees.appx" -AllowUnsigned
```

When your app is ready to be distributed, you should ensure that the package is signed. Be sure to remove the special OID, and ensure that the publisher name is the same as the certificate subject name.

## Related topics

* [Create hosted apps](/windows/uwp/launch-resume/hosted-apps)
