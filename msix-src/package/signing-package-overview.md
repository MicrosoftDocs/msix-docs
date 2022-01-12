---
description: This article describes signing requirements for Windows 10 apps. Signing is a required step in the process of creating an app package that can be deployed.
title: Sign a Windows 10 app package 
ms.date: 06/25/2020
ms.topic: article
keywords: windows 10, uwp, msix
---

# Sign a Windows 10 app package

App package signing is a required step in the process of creating a Windows 10 app package that can be deployed. Windows 10 requires all applications to be signed with a valid code signing certificate.

To successfully install a Windows 10 application, the package doesn't just have to be signed but also trusted on the device. This means that the certificate has to chain to one of the trusted roots on the device. By default, Windows 10 trusts certificates from most of the certificate authorities that provide code signing certificates.

|Topic| Description |
|:---|:---|
|[Prerequisites for signing](sign-app-package-using-signtool.md#prerequisites)| This section discusses the prerequisites required to sign the Windows 10 app package. | 
|[Using SignTool](sign-app-package-using-signtool.md#using-signtool)| This section discusses how to use SignTool from the Windows 10 SDK to sign the app package.|
|[Sign an MSIX package with Device Guard signing](./signing-package-device-guard-signing.md)| This section discusses how to sign your app with Device Guard signing.|

## Timestamping

It is highly recommended that **Timestamping** is used when signing your app with a certificate. Timestamping preserves the signature allowing the app package to be accepted by app deployment platform even after the certificate has expired. At the package inspection time, the timestamp allows for the package signature to be validated with respect to the time it was signed. This allows for packages to be accepted even after the certificate is no longer valid. Packages that are not timestamped will be evaluated against the current time and if the certificate is no longer valid, Windows will not accept the package. 

The following are the different scenarios around app signing with/out timestamping:

|Scenario|App is signed without timestamping | App is signed with timestamping |
|---|---------------------------------- | ------------------------------- |
| Certificate is valid |App will install | App will install |
| Certificate is invalid(expired) | App will fail to install | App will install as the authenticity of the cert was verified at signing by timestamping authority |

 > [!NOTE]
 > If the app is successfully installed on a device, it will continue to run even after the certificate expiry regardless of it being timestamped or not. 
 
 ## Package Integrity Enforcement
 
In addition to ensuring only trusted applications are installed on a device, an additional benefit of signing an MSIX package is that it enables Windows to enforce the integrity of your package and its contents after it is deployed on a device. By chaining to the [AppxBlockMap.xml](../overview.md#appxblockmapxml) and [AppxSignature.p7x](../overview.md#appxsignaturep7x) in a signed package, Windows is able to perform validation checks on the integrity of a package and its contents at runtime, and during Windows Defender scans. If a package is deemed to be tampered Windows will block application launch and kick-off a remediation workflow to get the package repaired or reinstalled. For packages not distributed through the Microsoft Store, package integrity is enforced if the package declares the [uap10:PackageIntegrity](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-packageintegrity) element and is deployed on Windows 2004 and later builds. Below is an example declaration of package integrity enforcement in the AppxManifest.xml:

```xml
<Package ...
xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"  
IgnorableNamespaces="uap10">
...
  <Properties>
    <uap10:PackageIntegrity>
      <uap10:Content Enforcement="on" />
    </uap10:PackageIntegrity>
  </Properties>
...
</Package>
```

## Device mode

Windows 10 allows users to select the mode in which to run their device on in the Settings app. The modes are Microsoft Store apps, Sideload apps, and Developer mode. 

**Microsoft Store apps** is the most secure as it only allows the installation of apps from the Microsoft Store. Apps in the Microsoft Store go through certification process to ensure that the apps are safe for use. 

**Sideload apps**  and **Developer mode** are more permissive of apps that are signed by other certificates as long as those certificates are trusted and chain to one of the trusted roots on the device. Only select Developer mode if you are a developer and building or debugging Windows 10 apps. More info about Developer mode and what it provides can be found [here](/windows/uwp/get-started/enable-your-device-for-development). 

> [!NOTE]
> Starting in Windows 10 version 2004, Sideload option is turned on by default. As a result, **Developer mode** is now a toggle. Enterprises can still turn off Sideloading via policy.
