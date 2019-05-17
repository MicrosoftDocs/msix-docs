# Signing Windows 10 App Package 

App package signing is a required step in the process of creating a Windows 10 app package that can be deployed. Windows 10 requires all applications to be signed with a valid code signing certificate. 

To successfully install a Windows 10 application, the package doesnt just have to be signed but also trusted on the device. This means that the certificate has to chain to one of the trusted roots on the device. Be default, Windows 10 trusts certificates from most of the certificate authority that provide code signing certs. 

## Timestamping 

Besides signing the app package with a certificate, another important characteristic that dictates the validity of the code signing certificate is **Timestamping**. Timestamping preserves the signature allowing the app package to accepted by app deployment platform even after the certificate has expired. At the package inspection time, the timestamp allows for the package signature to be validated with respect to the time it was signed. This allows for packages to be accepted even after the certificate is no longer valid. Packages that are not timestamped will be evaluated against the current time and if the certificate is no longer valid, Windows will not accept the package. 

The following are the different scenarios around app signing with/out timestamping:

App is signed without timestamping - will install until the certficate expires
App is signed without timestamping - will fail to install after the certificate expires
App is signed without timestamping - If app is already installed on a device, it will continue to run after certificate expiration. 

App is signed with timestamping - will install until the certificate expires
App is signed with timestamping - will install after the certificate expires as the authenticity of the cert was verified at signing by timestamping authority
App is signed with timestamping - If app is already installed on a device, it will continue to run after certificate expiration. 


## Device mode

Windows 10 allows users to select the mode in which to run their device on in the Settings app. The modes are Microsoft Store apps, Sideload apps, and Developer mode. 

**Microsoft Store apps** is the most secure as it only allows the installation of apps from the Microsoft Store. Apps in the Microsoft Store go through certification process to ensure that the apps are safe for use. 

**Sideload apps**  and **Developer mode** are more permissive of apps that are signed by other certificates as long as those certificates are trusted and chain to one of the trusted roots on the device. Only select Developer mode if you are a developer and building or debugging Windows 10 apps. More info about Developer mode and what it provides can be found [here](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development). 
