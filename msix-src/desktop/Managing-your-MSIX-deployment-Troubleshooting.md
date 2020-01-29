---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing your MSIX deployment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

## MSIX validation and troubleshooting
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
