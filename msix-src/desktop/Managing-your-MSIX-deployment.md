---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing your MSIX deployment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Managing your MSIX deployment 


Packaging your application is only half the battle.  Next you need to be able to deploy your application to your users.  How you deploy your application depends on who your customer is.  For enterprise customers, a software management system is used.  If you are targeting retail users, then deployment happens though the web and resources like the Windows Store.
This section will discuss deployment for both enterprise and retail markets.  It will provide links and tips and tricks to ensuring a successful experience.


## Know your target devices 
A key to successful distribution is knowing the devices your deployment is targeting.  Some enterprise companies have a single operating system distributed through out the organization.  Others have a mixed collection of hardware and operating systems.  Inorder to be succcesful in an mixed environment is to create a solution that will install easily on all operating systems while limiting the variations in installer technologies. 

All developers also need to know the minimum supported operating system they want to target.  Targeting the lowest common denominator of the operating system, may get you the best potential reach, but often earlier releases of the operating system may not support certain API calls your application is built using.   The following table will help all developers determine if they should us MSIXCore or MSIX. 


| Operating System | Version | MSIX Requirement |
| -----------------------|------------------------|------------------------|
|Windows 10 Client| 1709 (10.0.16299.0) or greater| MSIX Native Support|
|Windows 10 Server with Desktop Experience | 1709 (10.0.16299.0) or greater| Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 10 Client| 10.0.10240.0 – 10.0.15063.0 | Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 8 | | 		Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|
|Windows 7 | | 		Must use [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore)|

In addition, MSIX Native Support has improved with each release of Windows 10.  For a complete breakdown of MSIX features and the supporting operating system, see [LINKINKINKI BUGBUG ](..link link)


[Dian] add server stuff in table and update the version. Considerations for deployment – i.e SCCM, intune basically deployment technology and environment 

### MSIX Native support
As mentioned above, native support for MSIX was introduced in 1709 (10.0.16299.0) or greater of Windows.  This means that most MSIX packages when deployed to these platforms will just work.  It also means that users can double click MSIX packages to install the application as well. BUGBUG IS THIS A LIE?  IS DOUIBLE CLIKC ONLY 1809? RD - I believe this is supposted to be 1803+.

### MSIX Service support
[Windows services](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications), enable you to create long-running executable applications that run in their own Windows sessions.  If your MSIX package includes a Windows service, you will be limited to deploying that package to Windows 10, version 2004 and greater.  For more details on Windows Services support, please see https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services.

### MSIXCore
[MSIXCore](../MSIXCore/msixcore.md) expands the reach of your install.  IT developers will first deploy the [MSIX Core Installer](https://github.com/microsoft/msix-packaging/releases/) to the target platform.  Once installed, the IT developer can us Powershell or other scripting languages to deploy the MSIX package.  See [MSIXCore](https://docs.microsoft.com/windows/msix/msix-core/msixcore) for further details.

## Creating your MSIX
### Visual Studio
MSIX has been supported natively in [Visual Studio](https://visualstudio.microsoft.com/downloads/) since version Visual Studio 2017. MSIX is the modern way to deploy applications on Windows. It’s built to be safe, secure and reliable, and lets you and your customers take advantage of the new app model and the modern APIs that have been introduced in Windows 10, regardless of whether you intend to upload your apps to the Microsoft Store or sideload them onto computers in your enterprise.  

Most developers using [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) today will publish their installer using MSIX.  Creating a MSIX can be as simple as choosing Publish from the Visual Studio menu. For a deep dive into building MSIX file in Visual Studio as well as integrating this into your CI / CL build environment, see [MSIX: The Modern Way to Deploy Desktop Apps on Windows.](https://docs.microsoft.com/archive/msdn-magazine/2019/june/devops-msix-the-modern-way-to-deploy-desktop-apps-on-windows)

### Converting your installer
For those that are not writing new code, but instead deploying existing applications, the tool to help you do this is the [MSIX Packaging Tool.](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf?activetab=pivot:overviewtab)   With the use of this tool, you can convert your existing MSI or installer to MSIX for installation on supporting operating systems.  For details on the MSIX Packaging Tool, see [MSIX Packaging Tool.](https://docs.microsoft.com/windows/msix/packaging-tool/mpt-overview)

#### 	Bulk Operations 
Often developers need to convert more than one package and deploy it.  The [MSIX Toolkit] (https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) provides easy to use powershell scripts for batch conversion of your applications to MSIX. 


##	MSIX App Distribution
The MSIX packaging format can be delivered to client devices through the use of device and application management tools such as Microsoft Intune, and Microsoft System Center Configuration Manager. 

###	System Center Configuration Manager 
For details on the Microsoft Store for Business, seeMicrosoft System Center Configuration Manager (current branch) supports the deployment of MSIX applications to client devices through the Application model. As MSIX is a stanadardized installation packaging format, the details regarding the application (Publisher, Application Name, and Version) will be automatically retrieved and presented for review through the create application wizard within System Center Configuration Manager. Similarily, the install string and detection methods used with MSIX applications is consistent and automatically configured by the System Center Configuration Manager create application wizard.

When creating an application in System Center Configuration Manager, select application type: <b>Windows app package (*.appx, *.appxbundle, *.msix, *.msixbundle)</b>. For guidance on creating and deploying an application through [System Center Configuration Manager](https://docs.microsoft.com/configmgr/apps/get-started/create-and-deploy-an-application)

###	Microsoft Intune
Microsoft Intune supports the deployment of MSIX application to client devices through the client app model. As MSIX is a standardized installation packaging format, the details regarding the application (Application Name, Description, and Publisher) are automatically populated within the App information.

Installation of an MSIX application is standardized, as such, when adding a new <b>Line-of-business app</b> to Microsoft Intune, there is no requirement to configure the silent installation parameters required for install. For guidance on creating and deploying an application through Microsoft Intune: 

###	Web
For IIS distribution of a MSIX file, you can leverage the ms-appinstaller protocol.  For details on how to configure your IIS server to support MSIX app distribution, see [Distribute a Windows 10 app from an IIS server.] https://docs.microsoft.com/windows/msix/app-installer/web-install-iis

Bugbug should we dsicusss appisntaller?
https://docs.microsoft.com/en-us/windows/msix/app-installer/update-settings

### Microsoft Store
The [Microsoft App Store](https://www.microsoft.com/store/apps/windows) can be an excellent way to distribute your application.  It has been available to consumers since the launch of Windows 8 in late 2012. Users can browse, search and download apps or games for Windows.

For details on the Microsoft App Store and its capabilities, see [Microsoft App Store.](https://docs.microsoft.com/en-us/windows/uwp/publish/) 

To get started on publishing an app to the Microsoft App Store and its capabilities, see the [Partner Center](https://partner.microsoft.com/en-us/dashboard/home)

### Microsoft Store for Business
[Microsoft Store for Business](https://businessstore.microsoft.com/store) is a store specifically designed for Business and Education app distribution. You can use Microsoft Store to find, acquire, distribute, and manage apps for your organization or school.  For details on the Microsoft Store for Business, see [Microsoft Store for Business and Education.](https://docs.microsoft.com/microsoft-store/)

### App Center
[App Center](https://appcenter.ms/) enables you to automatically build your app, test it on real devices, and distribute it to beta testers.  App Center lets you ship apps more frequently, at higher-quality, and with greater confidence.  With App Center you can connect your repo and within minutes automate your builds, test on real devices in the cloud, distribute apps to beta testers, and monitor real-world usage with crash and analytics data. All in one place.

#### Known Issues

##	Updating MSIX
MSIX applications will contain a AppxBlockMap.xml file which contains a two dimensional list of information regarding files in the package. The first dimension provides the highlevel details (filename, and size) contained within the MSIX application. The second dimension provides a SHA2-256 hash block representation of each 64KB slice or each file. 

The Application installation file (*.msix) can be used for the purposes of installing and updating an application previously installed on a device. When a newer version of the MSIX application is installed, it will attempt to match the MSIX package family name, and the version of the MSIX application. If the family name matches, and the version of the newer MSIX application is greater than the currently installed application it will be updated.

As the [MSIX application is being updated](https://docs.microsoft.com/windows/msix/app-package-updates), the block hash of each file is compared against to determine if any changes have been made to a specific file. If a file has been identified as changed the file is replaced by the file contained within the newer MSIX application.

For the purpose of updating an MSIX application on a device, any supported device / application management solution which [supports the deployment or installation of an MSIX application](#MSIX-App-Distribution) can be used.


###	DISM and Provisioning

#### DISM
Deployment Image Servicing and Management (DISM.exe) is a command-line tool that can be used to service and prepare Windows images, including those used for Windows PE, Windows Recovery Environment (Windows RE) and Windows Setup. DISM can be used to service a Windows image (.wim) or a virtual hard disk (.vhd or .vhdx).   DISM comes built into Windows and is available through the command line or from Windows PowerShell. 

To learn more about using DISM with PowerShell, see [Deployment Imaging Servicing Management (DISM) Cmdlets in Windows PowerShell.](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

IT Pros can use DISM to preinstall and deploy their MSIX applications on their customers machines.

#### Provisioning
Windows provisioning makes it easy for IT Pros to configure end-user devices without imaging. Using Windows provisioning, IT Pros can easily specify desired configuration and settings required to enroll the devices into management and then apply that configuration to target devices in a matter of minutes.  

To learn more about provisioning, see [Provisioning Packages.](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages)

###### Provisioning MSIX packages
Starting in Windows version 1806 provisioning of MSIX packages was enabled.  IT Pros can now pre-install MSIXs.  Provisioned apps, will be installed to a central location, %ProgramFiles%\WindowsApps and will immediately be available to registetered users.  Users that do not have the MSIX registered to their account will not have access.

##### User Preferences are honored
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


## Trouble shooting
Though MSIX has a 99% successful install rate, sometimes you need to be able to trouble shoot an installation.

### Know the application
Understanding the application that you are installing can go a long ways to troubleshooting the user experience.  For example does the application the user is running require admin rights.  And is that what the user is struggling with.  By knowing the 

### Device Portal
Sometimes it is necessary to interact with the applciation experience to adequately
[Device Portal Desktop])https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)


The app deployment infrastructure provides logs for debugging in the Windows Event Viewer. These logs are found here: Application and Services Logs->Microsoft->Windows->AppxDeployment-Server

### Know the application
Understanding the application that you are installing and how it works can go a long ways to troubleshooting the user experience.  For example does the application have certain limiations that could be causing the issues?  Does the user have access to the resources needed by the applicaition?  

### Device Portal
Sometimes it is necessary to interact with the application in the user's environment to adequately understand the issue.  Windows provides a powerful tool, the [Device Portal Desktop])https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) which will allow you to connect to the device and interact with the application remotely.  


To learn more about provisioning, see [Provisioning Packages.](https://docs.microsoft.com/en-us/windows/configuration/provisioning-packages/provisioning-packages)


### Installation issues
When there are issues with the installation, you can investigate the artifacts provided by AppInstaller.  First, AppInstaller providea an error code failures.  If the error code for any failure.  If the error code is not sufficient to determine what is wrong, the AppInstaller also logs all interactions to the Event Viewer.  
You will find the logs here: Application and Services Logs->Microsoft->Windows->AppxDeployment-Server.

You can use the Event Viewer or Powershell to access those events. 
To learn more about how to view the events, see [Troubleshooting packaging, deployment, and query of Windows apps.](https://docs.microsoft.com/en-us/windows/win32/appxpkg/troubleshooting)


# BUGBUG  Kevin and Roy need to add Dian latest notes.

## Next steps

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
