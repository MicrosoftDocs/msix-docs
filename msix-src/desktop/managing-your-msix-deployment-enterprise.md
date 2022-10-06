---
description: This article provides all the details you need to manage deploying you MSIX applications in an enterprise environment.  This article is targeted at enterprise and IT Pros.
title: Distribute your MSIX in an enterprise environment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
---

#	MSIX App Distribution
The MSIX packaging format can be delivered to client devices through the use of device and application management tools such as Microsoft Intune, and Microsoft Endpoint Configuration Manager. 

Packaged apps can be installed using deployment tools, PowerShell, or by using [AppInstaller](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab). By using AppInstaller to install an MSIX packaged app, the user or IT Pro may select to right-click and install or double click the MSIX installer. This approach will prompt the user to select the **Install** button to initiate the installation and view the installation progress. Alternatively, by using available [PowerShell cmdlets](./powershell-msix-cmdlets.md) the installation and uninstallation of an MSIX packaged app can be performed silently.

##	Microsoft Endpoint Configuration Manager 

As MSIX is a standardized installation packaging format, the details regarding the application (Publisher, Application Name, and Version) will be automatically retrieved and presented for review through the create application wizard within Microsoft Endpoint Configuration Manager. Similarly, the install string and detection methods used with MSIX applications is consistent and automatically configured by the Microsoft Endpoint Configuration Manager create application wizard.

When creating an application in Microsoft Endpoint Configuration Manager, select application type: **Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)**. For guidance about how to create and deploy an application through Microsoft Endpoint Configuration Manager, see [create and deploy an application](/configmgr/apps/get-started/create-and-deploy-an-application).

## Microsoft Intune

Microsoft Intune supports the deployment of MSIX applications to client devices through the client app model. As MSIX is a standardized installation packaging format, the details regarding the application (Application Name, Description, and Publisher) are automatically populated within the App information.

Installation of an MSIX application is standardized. As such, when adding a new line-of-business app to Microsoft Intune, there is no requirement to configure the silent installation parameters required for install. For guidance about how to create and deploy an application through Microsoft Intune, see [Creating line of business apps in Intune](/mem/intune/apps/lob-apps-windows).

## Web (App Installer)

MSIX can be deployed with an IIS server.  If you add the ms-appinstaller protocol, it creates a much better install experience.  
For IIS distribution of a MSIX file, and how to configure your IIS server to support MSIX app distribution, see [Distribute a Windows 10 app from an IIS server.](../app-installer/web-install-iis.md)

## Microsoft Store for Business

[Microsoft Store for Business](https://businessstore.microsoft.com/store) is a store specifically designed for Business and Education app distribution. You can use Microsoft Store to find, acquire, distribute, and manage apps for your organization or school.  For details on the Microsoft Store for Business, see [Microsoft Store for Business and Education.](/microsoft-store/)

## App Center

[App Center](https://appcenter.ms/) enables you to automatically build your app, test it on real devices, and distribute it to beta testers.  App Center lets you ship apps more frequently, at higher-quality, and with greater confidence.  With App Center you can connect your repo and within minutes automate your builds, test on real devices in the cloud, distribute apps to beta testers, and monitor real-world usage with crash and analytics data. All in one place.

## Deployment Image Servicing and Management (DISM.exe) and Provisioning

### DISM
IT Pros can use the Deployment Image Servicing and Management (DISM) cmdlets to install, uninstall, and configure MSIX packages on a Windows image before deployment.  
To learn more about provisioning, see [Deployment Image Servicing and Management and Provisioning.](deploy-preinstalled-apps.md)

### Provisioning
IT Pros use provisioning to configure end-user devices without re-imaging.  IT Pros can pre-install MSIX packages on their end-users systems.
To learn more about provisioning, see [Deployment Image Servicing and Management and Provisioning.](deploy-preinstalled-apps.md)

## Managing your MSIX app

MSIX Packages have a comprehensive set of controls that IT Pros can use to control their installation.  IT Pros can dictate how and when MSIX apps can upgrade, downgrade or uninstall.  MSIX packages also can be limited with inbox Windows services like AppLocker and Group Policies. 

###	Prevent MSIX app installs through AppLocker

Supported in [AppLocker](/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview), is the ability to allow or deny MSIX applications to execute on a corporate device. This is done by defining rules based on the MSIX app attributes. These attributes include: publisher name, product name, file name, file version, file path and file hash. MSIX apps identified by these rules are then configured to allow or deny execution.

There are multiple methods in which AppLocker can be leveraged within an organization to control which apps may or may not be executed on a corporate device. For a full list see [Working with AppLocker Rules](/windows/security/threat-protection/windows-defender-application-control/applocker/working-with-applocker-rules).

### Manage access through Group Policy

Group Policies provide centralized management and configuration of operating systems, applications, and users' settings in an Active Directory environment. MSIX packages applications can read group policy registry keys and honor group policy settings.  
To learn more about MSIX support and limitations in group policy support, see [Group Policy and packaged apps.](../group-policy-msix.md)

### Manage MSIX updates

Configure the update behavior of the app by using the App Installer file.  IT Pros can define when a user gets updates to a MSIX and whether the update experience will be silent.  User may be required to update at launch or delayed.    

To learn more about Configuring a MSIX update schedule, see [Configure update settings in the App Installer file.](../app-installer/update-settings.md)

#### Downgrades

MSIX supports downgrading apps therefore the app does not require an uninstall prior to installing an older version of the same app. By specifying ForceUpdateFromAnyVersion the MSIX can be downgraded by a lower version. This is useful in the event that a serious bug has already been deployed.  

To learn more about ForceUpdateFromAnyVersion, see [Configure update settings in the App Installer file.](../app-installer/update-settings.md)

#### Critical Updates

Occasionally users ignore prompts to update their app.  With MSIX, IT Pros can force an update to an app, by marking as critical by specifying UpdateBlocksActivation.

To learn more about UpdateBlocksActivation, see [Configure update settings in the App Installer file.](../app-installer/update-settings.md)

## Uninstall

MSIX provides a robust install and uninstall story.  Since MSIX packages are are containerized packages, the unistall of the package will remove all application artifacts including all files written to  %ProgramFiles%WindowsApps as well as any system files in the AppData folder or registry settings created for the application.  The uninstall will not remove any user created files.
