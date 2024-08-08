---
title: Installing Windows 10 apps from a web page
description: In this topic, we'll describe the steps you need to take to allow users to install your apps directly from your web page.
ms.date: 12/13/2023
ms.topic: article
keywords: windows 11, windows 10, uwp, app installer, AppInstaller, sideload, related set, optional packages
ms.custom: RS5
---

# Installing Windows 10 apps from a web page

> [!IMPORTANT]
> This topic describes the *ms-appinstaller* URI (Uniform Resource Identifier) scheme (protocol), and how to use it. That URI scheme can be disabled by an IT professional (an administrator). To enadisable *ms-appinstaller* on your network, set the Group Policy **EnableMSAppInstallerProtocol** (/windows/client-management/mdm/policy-csp-desktopappinstaller) to disabled (see [Policy CSP - DesktopAppInstaller](/windows/client-management/mdm/policy-csp-desktopappinstaller#enablemsappinstallerprotocol)). If the Group Policy **EnableMSAppInstallerProtocol** is set to enabled, or if it isn't specified, then *ms-appinstaller* is enabled.
>
> When the *ms-appinstaller* URI scheme is disabled, App Installer will *not* be able to install an app directly from a web server (which is what this topic is about). In that case, the user *will* need to download the app first. So update the link on your website by removing `'ms-appinstaller:?source='` so that the MSIX package or `.appinstaller` file will be downloaded. That might increase the download size for some packages. The user can then install the package by using App Installer.

Typically, an app needs to be locally available on a device before it can be installed with the App Installer. For the web scenario, this means that the user must download the app package from the web server, after which it can be installed with App Installer. This is inefficient and wastes disk space, which is why App Installer now has built in features to streamline the process.

App Installer can install an app directly from a web server. When the user clicks on an app package hosted web link, App Installer is invoked automatically. The user is then taken to the app info view in App Installer and is then one click away from engaging directly with the app.

The direct app install is only available in the Windows 10 Fall Creators Update and newer. Previous versions of Windows (going back to the Windows 10 Anniversary Update) will be supported by the [web install experience on previous versions of Windows 10](#web-install-experience). This experience is not as fluid as the direct app install, but it provides significant improvements to the existing app install procedure.

> [!NOTE]
> App Installer version must be greater than 1.0.12271.0 to support this feature.

## Protocol Activation Scheme

In this mechanism, App Installer registers with the operating system for a protocol activation scheme. When user clicks on a web link, the browser checks with the OS for apps that are registered to that web link. If the scheme matches the protocol activation scheme specified by App Installer, then App Installer is invoked. It's important to note that this mechanism is browser independent. This is beneficial to site administrators, for example, who don't need to consider web browser differences while incorporating this into a webpage.

### Requirements for protocol activation scheme

1. Web servers need to have support for byte range requests (HTTP/1.1)
    - Servers that support HTTP/1.1 protocol should have support for byte range requests
2. Web servers will need to know about the Windows 10 app package content types
    - Here's how to declare the new content types as part of [web config file](web-install-IIS.md#step-7---configure-the-web-app-for-app-package-mime-types)

### How to enable this on a webpage

App developers who want to host app packages on their web sites need to follow this step:

Prefix your app package URIs with the activation scheme `'ms-appinstaller:?source='` that App Installer is registered to when referencing them on your webpage. See the example for **MyApp Web Page** for details.
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.msix"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.msixbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

> [!NOTE]
> By prefixing the link to the Windows app, or AppInstaller file with `ms-appinstaller:?source=''` client devices will launch the Desktop App Installer, with details pertaining to the Windows app. MIME Types must be configured on the Web Server as this information will be shared with the Desktop App Installer informing of the file type and it's file type association.

It is required that MIME-Types be configured for the Windows apps and AppInstaller files that will be shared from your website. By including the MIME Types, the Desktop App Installer will quickly identify the file association and launch the information page with next steps. If not included, the Desktop App Installer must determine the file association which can negatively impact how quickly the Desktop App Installer will interpret the information and launch the Windows app installer. The only MIME-Types that are required to be configured on your Web Server are of the file types that will be hosted on your website.

If the Windows app installation media is hosted on a file share, and linked to from the website then MIME-Types need not be configured on the Web Server. 

| File Extension | MIME Type                |
|----------------|--------------------------|
| .msix          | application/msix         |
| .appx          | application/appx         |
| .msixbundle    | application/msixbundle   |
| .appxbundle    | application/appxbundle   |
| .appinstaller  | application/appinstaller |

For more information on how to configure the MIME types, please visit [Distribute a Windows 10 App from an IIS Server](./web-install-iis.md#step-7---configure-the-web-app-for-app-package-mime-types).

## Signing the app package

For users to install your app, you will need to sign the app package with a trusted certificate. You can use a third party paid certificate from a trusted certification authority to sign your app package. If a third party certificate is used, the user will need to have their device in either sideload or developer mode to install and run your app.

If you are deploying an app to employees within an enterprise, you can use an enterprise issued certificate to sign the app. It's important to note that the enterprise certificate must be deployed to any devices which the app will be installed on. For more information on deploying enterprise apps, see [Enterprise app management](/windows/client-management/mdm/enterprise-app-management).

## Web install experience on previous versions of Windows 10<a name="web-install-experience"></a>

Invoking App Installer from the browser is supported on all versions of Windows 10 where App Installer is available (starting with the Anniversary Update). However, the functionality to install directly from the web without the need to download the package first is only available on the Windows 10 Fall Creators Update.  

Users of previous versions of Windows 10 (with App Installer available) can also take advantage of web install of Windows 10 apps via App Installer, but will have a different user experience. When these users click the web link, App Installer will prompt to **Download** the package instead of **Install**. After download, App Installer will initiate the launch of the downloaded package automatically. One more click on **Install**, and the app is ready for use.

Although this flow isn't quite as seamless as the direct install on Windows 10 Fall Creators Update, users can still quickly engage with the app. Additionally, with this flow the user doesn't have to worry about app package files unnecessarily taking up space in drives. App Installer efficiently manages space by downloading the package to its app data folder and clearing packages when they are no longer needed.

Here's a quick comparison of the Windows 10 Fall Creators update version of App Installer and the previous version of App Installer:

| App Installer, Latest Version | App Installer, Previous Version |
|------------------------------|----------------------------------|
| App Installer shows app info before the download starts | Browser prompts the user to choose to download  |
| App Installer performs the download | User has to manually initiate the launch of the app package |
| After package download, App Installer automatically launches the app package | User must click **Install** and manually launch the app package |
| App Installer will take care of disposal of downloaded packages | User must manually delete the downloaded files |

On versions prior to the Windows 10 Fall Creators Update, App Installer cannot directly install an app from the web. On these versions, App Installer can only install app packages that are locally available. Instead, App Installer will download the package and require the user to double click the downloaded package to install.

## App Installer Security

With version <bugbug> of the App Installer, the following security measures have been added:
* Internet Zone validation
* SmartScreen Validation

### Internet Zone Validation
Prior to accessing the domain referenced by the the *ms-appinstaller* URI scheme, the App Installer will verify that the domain is allowed by the IT Professional. If the domain has been restricted, the App Installer will present an error to the user.

### Smart Screen Validation
If the domain referenced by the *ms-appinstaller* URI scheme is allowed, the App Installer will validate the URI with [Smart Screen](https://learn.microsoft.com/en-us/windows/security/operating-system-security/virus-and-threat-protection/microsoft-defender-smartscreen)>. URIs that fail the reputation check will present the user with an error.

See [App Installer security features](./app-installer-security-features.md) for more information.