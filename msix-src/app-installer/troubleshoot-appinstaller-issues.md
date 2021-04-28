---
title: Troubleshoot installation issues with the App Installer file
description: Common issues when sideloading applications with the App Installer file.
ms.date: 4/28/2021
ms.topic: article
keywords: windows 10, uwp, app installer, AppInstaller, sideload
ms.localizationpriority: medium
ms.custom: RS5
---

# Troubleshoot installation issues with the App Installer file

If you find any issues when installing an application from the App Installer file, this topic will provide some troubleshooting guidance that may help.

## Prerequisites

To be able to sideload apps in Windows 10, the user device must satisfy the next requirements:

**Windows 10:**
- The certificate used to sign the package must be trusted by the device. See the **Trusted certificates** section below for more details.
- The Windows 10 version must support the `.appinstaller` file schema and the distribution protocol.

**Windows 10 1909 and earlier:**
- The device must be enabled for Developer Mode or Sideloading apps. See [Enable your device for development](/windows/uwp/get-started/enable-your-device-for-development) to learn more.

## Common issues

There are some common issues when sideloading an application for first time in the user machine. The next few sections describe the most frequent issues and their solutions.

### Windows version

Each Windows 10 release improves on the sideloading experience, in the table below you will find which features are available in each major release. If you try to sideload an app using a method not supported in your version of Windows 10, you will get a deployment error.

| Version | Sideload Notes |
|---------|----------------|
| Build 17134 (April 2018 Update, version 1803)    | The `.appinstaller` file can be accessed over UNC/Share folders. Configurable update checks are also available. |
| Build 16299 (Fall Creators Update, version 1709) | Introduced the `.appinstaller` file to provide automatic updates to your app. This version only supports HTTP endpoints. Update checks are not configurable and happens each 24 hours. |
| Build 15063 (Creators Update, version 1703)      | The App Installer app is able to download app dependencies (only in release mode) from the Store. |
| Build 14393 (Anniversary Update, version 1607)   | Introduced the App Installer app to install .appx and .appxbundle files, .appinstaller file is not supported. |
| Build 10586 (November Update, version 1511)      | Sideload is only available through PowerShell using the [Add-AppxPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) command. |
| Build 10240 (Windows 10, version 1507)           | Sideload is only available through PowerShell using the [Add-AppxPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) command. |

### Trusted certificates

App packages must be signed with a certificate that is trusted by the device. Certificates provided by common Certificate Authorities are trusted by default in the Windows operating system.

However, if the certificate used to sign an app package is not trusted, or is a locally-generated/self-signed certificate used during development, the app installer may report that the package is untrusted and will prevent it from being installed:

![MSIX signed with missing or untrusted Cert](..\images\msix-bad-cert.png)

To solve this issue, a user with local administrator rights to the device must use the **Computer Certificates** tool to import the certificate into one of the following containers:

1. Local Computer: Trusted People
2. Local Computer: Trusted Root Authorities (not recommended)

>[!IMPORTANT]
> **Do not import package signing certificates into the User Certificate store**. The App Installer does not search User Certificates when verifying package identity.

The Computer Certificates management tool can be easily found by searching from the Start Menu:

![Find the local Computer Certificates tool via the Start Menu](..\images\start-comp-cert.png)

Once the signing certificate is successfully imported, re-running the app installer will show that the package is trusted and can be installed:

![MSIX signed with a trusted Cert](..\images\msix-good-cert.png)

### Dependencies not installed

Windows 10 applications can have framework dependencies based on the application platform used to generate the app. If you are using C# or VB, the app will require the .NET Runtime and .NET framework packages. C++ applications require the VCLibs.

>[!IMPORTANT]
> If the app package is built in Release mode configuration, the framework dependencies will be obtained from the Microsoft Store. However, if the app is built in Debug mode configuration, the dependencies will be obtained from the location specified in the `.appinstaller` file.

### Files not accessible

When installing from an HTTP endpoint, it is important to verify that all files are accessible with the correct MIME type. The easiest method to verify these files is by following the links provided in the HTML page generated by Visual Studio. You must check these files:

- `.appinstaller` file, available as an `application/xml`
- `.appx` and `.appxbundle` files, available as `application/vns.ms-appx`

## Isolate App Installer app issues

If the App Installer cannot install the app, these steps will help identify the installation issue.

### Verify app package file installation

- Download the app package file to a local folder and try to install it using the [Add-AppxPackage](/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell command.

- Download the `.appinstaller` file to a local folder and try to install it using the `Add-AppxPackage -Appinstaller` PowerShell command.

### App Installer event logs

The app deployment infrastructure emits logs that are often useful for debugging installation issues via the Windows Event Viewer: `Application and Services Logs -> Microsoft -> Windows -> AppxDeployment-Server`
