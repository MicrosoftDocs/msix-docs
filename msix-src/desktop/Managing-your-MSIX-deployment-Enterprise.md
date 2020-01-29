---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing your MSIX deployment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

#	MSIX App Distribution
The MSIX packaging format can be delivered to client devices through the use of device and application management tools such as Microsoft Intune, and Microsoft System Center Configuration Manager. 

##	System Center Configuration Manager 
For details on the Microsoft Store for Business, seeMicrosoft System Center Configuration Manager (current branch) supports the deployment of MSIX applications to client devices through the Application model. As MSIX is a stanadardized installation packaging format, the details regarding the application (Publisher, Application Name, and Version) will be automatically retrieved and presented for review through the create application wizard within System Center Configuration Manager. Similarily, the install string and detection methods used with MSIX applications is consistent and automatically configured by the System Center Configuration Manager create application wizard.

When creating an application in System Center Configuration Manager, select application type: <b>Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)</b>. For guidance on creating and deploying an application through [System Center Configuration Manager](https://docs.microsoft.com/configmgr/apps/get-started/create-and-deploy-an-application)

##	Microsoft Intune
Microsoft Intune supports the deployment of MSIX application to client devices through the client app model. As MSIX is a standardized installation packaging format, the details regarding the application (Application Name, Description, and Publisher) are automatically populated within the App information.

Installation of an MSIX application is standardized, as such, when adding a new <b>Line-of-business app</b> to Microsoft Intune, there is no requirement to configure the silent installation parameters required for install. For guidance on creating and deploying an application through Microsoft Intune: 

##	Web
For IIS distribution of a MSIX file, you can leverage the ms-appinstaller protocol.  For details on how to configure your IIS server to support MSIX app distribution, see [Distribute a Windows 10 app from an IIS server.] https://docs.microsoft.com/windows/msix/app-installer/web-install-iis

Bugbug should we dsicusss appisntaller?
https://docs.microsoft.com/en-us/windows/msix/app-installer/update-settings

## Microsoft Store
The [Microsoft App Store](https://www.microsoft.com/store/apps/windows) can be an excellent way to distribute your application.  It has been available to consumers since the launch of Windows 8 in late 2012. Users can browse, search and download apps or games for Windows.

For details on the Microsoft App Store and its capabilities, see [Microsoft App Store.](https://docs.microsoft.com/en-us/windows/uwp/publish/) 

To get started on publishing an app to the Microsoft App Store and its capabilities, see the [Partner Center](https://partner.microsoft.com/en-us/dashboard/home)

## Microsoft Store for Business
[Microsoft Store for Business](https://businessstore.microsoft.com/store) is a store specifically designed for Business and Education app distribution. You can use Microsoft Store to find, acquire, distribute, and manage apps for your organization or school.  For details on the Microsoft Store for Business, see [Microsoft Store for Business and Education.](https://docs.microsoft.com/microsoft-store/)

## App Center
[App Center](https://appcenter.ms/) enables you to automatically build your app, test it on real devices, and distribute it to beta testers.  App Center lets you ship apps more frequently, at higher-quality, and with greater confidence.  With App Center you can connect your repo and within minutes automate your builds, test on real devices in the cloud, distribute apps to beta testers, and monitor real-world usage with crash and analytics data. All in one place.

##	DISM and Provisioning

### DISM
Deployment Image Servicing and Management (DISM.exe) is a command-line tool that can be used to service and prepare Windows images, including those used for Windows PE, Windows Recovery Environment (Windows RE) and Windows Setup. DISM can be used to service a Windows image (.wim) or a virtual hard disk (.vhd or .vhdx).   DISM comes built into Windows and is available through the command line or from Windows PowerShell. 

To learn more about using DISM with PowerShell, see [Deployment Imaging Servicing Management (DISM) Cmdlets in Windows PowerShell.](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

IT Pros can use DISM to preinstall and deploy their MSIX applications on their customers machines.

### Provisioning
Windows provisioning makes it easy for IT Pros to configure end-user devices without imaging. Using Windows provisioning, IT Pros can easily specify desired configuration and settings required to enroll the devices into management and then apply that configuration to target devices in a matter of minutes.  

To learn more about provisioning, see [Provisioning Packages.](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages)

#### Provisioning MSIX packages
Starting in Windows version 1806 provisioning of MSIX packages was enabled.  IT Pros can now pre-install MSIXs.  Provisioned apps, will be installed to a central location, %ProgramFiles%\WindowsApps and will immediately be available to registetered users.  Users that do not have the MSIX registered to their account will not have access.

#### User Preferences are honored
Because provisioning respects the users settings, there are some consequences you should be aware of.  If a user uninstalls a provisioned app, the app will not be re-installed if you re-provision an update.  The user chose to uninstall the app, so that setting is honored.  

Note: In Windows version 1903, a provisioned app will automatically overide the users setting and reinstall the app when re-provisioned.

You should also be careful to prevent the users from installing and updating different versions of the provisioned MSIX package.  If the user upgrades or downgrades a provisioned app, by installing the same package from the store, you may get unexpected results. 

For more details on MSIX provisioning, see [Create Windows applications in Configuration Manager.](https://docs.microsoft.com/en-us/configmgr/apps/get-started/creating-windows-applications)


## Managing your MSIX Application  
Organizations leveraging MSIX apps have the ability to manage the app installation capability, as well as controls how the app functions while installed on a client device.

###	Prevent MSIX app installs through AppLocker
Supported in [AppLocker](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview), is the ability to allow or deny MSIX applications to execute on a corporate device. This is done by defining rules based on the MSIX app attributes. These attributes include: publisher name, product name, file name, file version, file path and file hash. MSIX apps identified by these rules are then configured to allow or deny execution.

There are multiple methods in which AppLocker can be leveraged within an organization to control which apps may or may not be executed on a corporate device. For a full list see [Working with AppLocker Rules](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/working-with-applocker-rules).

### Group Policy

### Force update
The app deployment infrastructure provides logs for debugging in the Windows Event Viewer. These logs are found here: Application and Services Logs->Microsoft->Windows->AppxDeployment-Server
### delay registration
### rollback
### force takedown 

## Uninstall [kevinla]
MSIX provides a robust install and uninstall story.  Since MSIX packages are are containerized packages, the unistall of the package will remove all application artifacts including all files written to  %ProgramFiles%WindowsApps as well as any system files in the AppData folder or registry settings created for the application.  The uninstall will not remove any user created files.

