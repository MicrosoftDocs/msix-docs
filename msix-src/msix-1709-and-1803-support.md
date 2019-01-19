# MSIX support on builds 1709 and 1803

This article summarizes the support for MSIX with our latest updates as of 1/22/2019.

By popular demand, we have added support for MSIX on more Windows versions. Most notably, this covers versions 1709 and 1803. This support allows users to deploy MSIX packages on earlier Windows versions, while taking advantage of all benefits of MSIX, including containerization and security through certificates.

Below, we discuss the caveats for MSIX support on the earlier OS versions.

## Auto elevation:
Admins using builds earlier than 1809 cannot auto-elevate MSIX apps. On these earlier builds, apps installed through MSIX can still be run with elevated privileges, however, the user must manually allow that. One way to do that is by right-clicking on the app's tile and selecting the "Run as administrator" option.

## Store support:
The minimum supported OS version of an MSIX is listed in the manifest file of the package as MinVersion in the TargetDeviceFamily element. For example an MSIX package may list MinVersion="10.0.17701.0" as the min supported version, which means that the MSIX can run on this and later versions of the OS.

For MSIX packages whose minimum supported OS version is listed as 17701 (corresponding to version 1809) and later, an MSIX package can be deployed in many ways. For example MSIX packages can be deployed through the store,  store for business, Intune, SCCM, by clicking on the MSIX package directly, by clicking on a .appinstaller file which references the MSIX package, or through powershell.. 

However, on versions between 1709 and 1808 inclusive (corresponding to builds 16299 to 17700) , MSIX package deployment is more limited. Specifically, MSIX packages cannot be deployed through the store or store for business. However, they can still be deployed through powershell.


## Packaging & signing: 
Currently to package and sign an MSIX, you need the MSIX packaging tool or Visual Studio. Both tools require the 1809 SDK or later. In earlier SDKs packaging and/or signing MSIX is not supported.
 
## Verifying signing: 
As mentioned earlier, we enforce proper signing of MSIX packages on all Windows versions 1709 and later. However, on the older Windows versions (1709 and 1803), users need a few extra steps to verify the signature for themselves. 

On Windows 10 versions 1809 and later, the MSIX user can verify the app's signature either from PowerShell or through the properties of the MSIX package. 

However, on versions between 1709 and 1808 inclusive, users cannot verify the signature from the MSIX package's properties, unless the 1809 SDK is installed. If the user has the 1809 SDK on a device with Windows 1709 through 1808, the signature can be verified through SDK tools from PowerShell. 

##  MSIX double-click support: 
One of the benefits of deploying and MSIX on version 1809 and later is that the user can install the package by clicking on it. This causes our OS component to show some UI to the user and guide them through the app installation.

With our latest update to the App Installer - the component that guides the user through installation -  this functionality is available on versions 1709 and 1803 as well. 
