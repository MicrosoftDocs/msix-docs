#Signing your MSIX Package in Visual Studio

Digital Signing The packaging project includes a default password-protected personal information exchange (PFX) format file that you probably want to replace with your own. If your enterprise doesn’t provide you with a code-signing certificate, you can either buy one from a trusted authority or create a self-signed certificate. There’s a “Create test certificate” option and an import wizard in Visual Studio, which you’ll find if you open the Package.appxmanifest file in the default app manifest designer and look under the Packaging tab. If you’re not that into wizards and dialogs, you can use the New-SelfSignedCertificate PowerShell cmdlet to create a certificate:
```
 > New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyCompany,
  O=MyCompany, L=Stockholm, S=N/A, C=Sweden" -KeyUsage DigitalSignature
    -FriendlyName MyCertificate -CertStoreLocation "Cert:\LocalMachine\My"
      -TextExtension @('2.5.29.37={text}1.3.6.1.5.5.7.3.3',
        '2.5.29.19={text}Subject Type:End Entity')
```

The cmdlet outputs a thumbprint (like the A27…D9F here) that you can pass to another cmdlet, Move-Item, to move the certificate into the trusted root certification store:

```
>Move-Item Cert:\LocalMachine\My\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F
  -Destination Cert:\LocalMachine\Root
```
Again, you need to install the certificate into this store on all computers where you intend to install and run the packaged app. You also need to enable sideloading of apps on these devices. On an unmanaged computer, this can be done under Update & Security | For Developers in the Settings app. On a device that’s managed by an organization, you can turn on sideloading by pushing a policy with a mobile device management (MDM) provider.

The thumbprint can also be used to export the certificate to a new PFX file using the Export-PfxCertificate cmdlet:

```
>$pwd = ConvertTo-SecureString -String secret -Force -AsPlainText
>Export-PfxCertificate -cert
  "Cert:\LocalMachine\Root\A27A5DBF5C874016E1A0DEBF38A97061F6625D9F"
    -FilePath "c:/<SolutionFolder>/Msix/certificate.pfx" -Password $pwd
```
Remember to tell Visual Studio to use the generated PFX file to sign the MSIX package by selecting it under the Packaging tab in the designer, or by manually editing the .wapproj project file and replacing the values of the <PackageCertificateKeyFile> and <PackageCertificateThumbprint> elements.
