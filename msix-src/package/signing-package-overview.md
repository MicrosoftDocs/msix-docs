# Signing Windows 10 App Package 

App package signing is a required step in the process of creating a Windows 10 app package that can be deployed. Windows 10 requires all applications to be signed with a valid code signing certificate. 

To successfully install a Windows 10 application, the package doesnt just have to be signed but also trusted on the device. This means that the certificate has to chain to one of the trusted roots on the device. Be default, Windows 10 trusts certificates from most of the certificate authority that provide code signing certs. 

## Device mode

## Timestamping 

