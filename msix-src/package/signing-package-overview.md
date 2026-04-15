---
description: Learn how to sign an MSIX app package for deployment on Windows, including signing options for development, testing, and production.
title: Sign an MSIX package
ms.date: 04/14/2026
ms.topic: article
keywords: windows, msix, signing, certificate, artifact signing, trusted signing, signtool, winapp cli
---

# Sign an MSIX package

App package signing is a required step in the process of creating an MSIX package that can be deployed. Windows requires MSIX packages to be signed with a valid code signing certificate.

To successfully install a Windows application, the package doesn't just have to be signed but also trusted on the device. This means that the certificate has to chain to one of the trusted roots on the device. By default, Windows trusts certificates from most certificate authorities that provide code signing certificates.

Additionally, if you are creating an MSIX bundle, there is no need to sign all the packages in the bundle individually. Only the bundle needs to be signed; the signature covers the packages inside the bundle.

## Signing options

Choose a signing approach based on your scenario:

| Scenario | Option | Cost |
|---|---|---|
| Development and local testing | Self-signed certificate | Free |
| Production distribution (recommended) | [Azure Artifact Signing](/azure/trusted-signing/) (formerly Trusted Signing) | Basic: ~$10/month |
| Production distribution (alternative) | OV code signing certificate from a CA | $300–500/year |
| Microsoft Store distribution | Signed by the Store on submission | Free |

> [!NOTE]
> **Azure Artifact Signing** (formerly known as Trusted Signing) is Microsoft's managed code signing service and is the recommended option for production MSIX signing. Key characteristics:
>
> - **Instant SmartScreen reputation**: Reputation is tied to your verified identity, not a certificate lifetime — so new builds get immediate trust.
> - **Short-lived certs**: A new certificate is issued daily, and each certificate remains valid for about 3 days, enabling time-precise revocation if needed.
> - **CI/CD ready**: Supports GitHub Actions (`azure/trusted-signing-action`) and Azure DevOps out of the box.
>
> **Eligibility for Public Trust certificates**: Available to organizations in the USA, Canada, the European Union, and the United Kingdom, and to individual developers in the USA and Canada. Organizations must have a verifiable tax history of three or more years. See [Important information for identity validation](/azure/trusted-signing/quickstart#important-information-for-public-identity-validation).
>
> **Signing with SignTool requires extra setup**: SignTool works with Artifact Signing only when you use the Artifact Signing Client Tools, which include the required dlib plugin and .NET 8 runtime. You must also provide a `metadata.json` file with your account endpoint and certificate profile. A standard Windows SDK SignTool invocation by itself won't work with Artifact Signing. The easiest installation is:
>
> ```
> winget install -e --id Microsoft.Azure.ArtifactSigningClientTools
> ```
>
> See [Set up SignTool with Artifact Signing](/azure/trusted-signing/how-to-signing-integrations#set-up-signtool-to-use-artifact-signing) for the complete setup.
>
> **AzureSignTool** is a separate community tool for signing with certificates stored in Azure Key Vault. It does *not* support Artifact Signing — the two are distinct services. For Azure Key Vault-based signing in Visual Studio, see [Sign packages with Azure Key Vault](../desktop/sign-with-akv-cert.md).

## WinApp CLI

The [WinApp CLI](/windows/apps/dev-tools/winapp-cli) provides convenient commands for development signing:

- `winapp cert generate` — create a self-signed certificate for development
- `winapp sign` — sign an MSIX package or executable with a certificate
- `winapp tool signtool` — access SignTool directly from the Windows SDK

## Signing topics

|Topic| Description |
|:---|:---|
|[Prerequisites for signing](sign-app-package-using-signtool.md#prerequisites)| Prerequisites required to sign an app package. | 
|[Using SignTool](sign-app-package-using-signtool.md#using-signtool)| How to use SignTool from the Windows SDK to sign an app package.|
|[Sign packages with Azure Key Vault](../desktop/sign-with-akv-cert.md)| How to sign packages using a certificate stored in Azure Key Vault from Visual Studio.|
|[Sign an MSIX package with Device Guard signing](./signing-package-device-guard-signing.md)| How to sign your app with Device Guard signing.|
|[Creating unsigned packages for testing](./unsigned-package.md)| How to create an unsigned MSIX package for testing.|
|[Azure Artifact Signing](/azure/trusted-signing/)| Microsoft's managed signing service (formerly Trusted Signing) for production MSIX packages.|

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
