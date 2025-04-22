---
title: Create a certificate for package signing
description: Create and export a certificate for app package signing with PowerShell tools.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
---

# Create a certificate for package signing

This article explains how to create and trust a certificate for app package signing using PowerShell tools (for CMD tools, see [here](/windows/win32/appxpkg/how-to-create-a-package-signing-certificate)). It's recommended that you use Visual Studio for [packaging UWP apps](packaging-uwp-apps.md) and [packaging desktop apps](../desktop/desktop-to-uwp-packaging-dot-net.md), but you can still package an app manually if you did not use Visual Studio to develop your app.

## Prerequisites

- **A packaged or unpackaged app**  
An app containing an AppxManifest.xml file. You will need to reference the manifest file while creating the certificate that will be used to sign the final app package. For details on how to manually package an app, see [Create an app package with the MakeAppx.exe tool](create-app-package-with-makeappx-tool.md).

- **Public Key Infrastructure (PKI) Cmdlets**  
You need PKI cmdlets to create and export your signing certificate. For more information, see [Public Key Infrastructure Cmdlets](/powershell/module/pki).

## Create a self-signed certificate

A self-signed certificate is useful for testing your app before you're ready to publish it to the Store. Follow the steps outlined in this section to create a self-signed certificate.

> [!NOTE]
> When you create and use a self-signed certificate only users who install and trust your certificate can run your application. This is easy to implement for testing but it may prevent additional users from installing your application. When you are ready to publish your application we recommend that you use a certificate issued by a trusted source. This system of centralized trust helps to ensure that the application ecosystem has levels of verification to protect users from malicious actors.

### Determine the subject of your packaged app  

To use a certificate to sign your app package, the "Subject" in the certificate **must** match the "Publisher" section in your app's manifest.

For example, the "Identity" section in your app's AppxManifest.xml file should look something like this:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

The "Publisher", in this case, is "CN=Contoso Software, O=Contoso Corporation, C=US" which needs to be used for creating your certificate.

### Use **New-SelfSignedCertificate** to create a certificate

Use the **New-SelfSignedCertificate** PowerShell cmdlet to create a self signed certificate. **New-SelfSignedCertificate** has several parameters for customization, but for the purpose of this article, we'll focus on creating a simple certificate that will work with **SignTool**. For more examples and uses of this cmdlet, see [New-SelfSignedCertificate](/powershell/module/pki/new-selfsignedcertificate).

Based on the AppxManifest.xml file from the previous example, you should use the following syntax to create a certificate. In an elevated PowerShell prompt:

```powershell
New-SelfSignedCertificate -Type Custom -KeyUsage DigitalSignature -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}") -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -FriendlyName "Your friendly name goes here"
```

Note the following details about some of the parameters:

- **KeyUsage**: This parameter defines what the certificate may be used for. For a self-signing certificate, this parameter should be set to **DigitalSignature**.

- **TextExtension**: This parameter includes settings for the following extensions:

  - Extended Key Usage (EKU): This extension indicates additional purposes for which the certified public key may be used. For a self-signing certificate, this parameter should include the extension string **"2.5.29.37={text}1.3.6.1.5.5.7.3.3"**, which indicates that the certificate is to be used for code signing.

  - Basic Constraints: This extension indicates whether or not the certificate is a Certificate Authority (CA). For a self-signing certificate, this parameter should include the extension string **"2.5.29.19={text}"**, which indicates that the certificate is an end entity (not a CA).

After running this command, the certificate will be created and added to the User Personal certificate store. The result of the command will also produce the certificate's thumbprint.  

You can view your certificate in a PowerShell window by using the following commands:

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

This will display all of the certificates in the User Personal certificate store.

In order to install an app signed with this certificate, the certificate must be imported into the Local Machine Trusted People certificate store.

## Export the certificate to a PFX file

In order to import the newly created certificate into the Local Machine Trusted People certificate store, you need to first export it to a Personal Information Exchange (PFX) file using the **Export-PfxCertificate** cmdlet.

When using **Export-PfxCertificate**, you must either create and use a password or use the "-ProtectTo" parameter to specify which users or groups can access the file without a password. Note that an error will be displayed if you don't use either the "-Password" or "-ProtectTo" parameter. "-Password" is recommended for general usage while "-ProtectTo" is useful when your user account is backed by a domain controller.

### Password usage

```powershell
$password = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $password
```

### ProtectTo usage

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

## Import the certificate to the Local Machine Trusted People store

Now that you've exported the certificate to a PFX file, you can import it into the Local Machine Trusted People store using the **Import-PfxCertificate** cmdlet from an admin PowerShell session.

```powershell
Import-PfxCertificate -CertStoreLocation "Cert:\LocalMachine\TrustedPeople" -Password $password -FilePath <FilePath>.pfx
```

Now that the certificate is trusted, you're ready to sign your app package with **SignTool**. For the next step in the manual packaging process, see [Sign an app package using SignTool](sign-app-package-using-signtool.md).

## Security considerations

By adding a certificate to [local machine certificate stores](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), you affect the certificate trust of all users on the computer. It is recommended that you remove those certificates when they are no longer necessary to prevent them from being used to compromise system trust.
