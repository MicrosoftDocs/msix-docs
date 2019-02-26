---
title: Troubleshooting Tips | Microsoft Docs
description: MSIX Packaging Tool Troubleshooting tips and known issues 
author: pezan
ms.author: pezan
ms.date: 12/11/2018
ms.topic: article
keywords: msix packaging tool, known issues
ms.localizationpriority: medium
ms.custom: RS5
---
# Ensure that your machine is set up for conversion 
We provide a list of best practices here. A couple of considerations regarding the drivers the following

# Known Issues
#### MSIX Packaging Tool driver considerations
MSIX Packaging Tool driver is delivered as a [Feature on Demand(FOD)](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) package from Windows Update and will fail to install if WU service is disabled on the machine or Windows Insider flight ring settings do no match the OS build of the machine. 

#### Installing MSIX Packaging Tool driver FOD on Windows Insider builds
If you are having issues installing the driver on an insider build of Windows:
- Navigate to Settings, Updates & Security, Windows Insider Program.
- If you see a message that Insider preview build settings need attention, click on the “Fix me” button to log in again. You might have to go to Windows Update page and check for update before settings change takes effect. 
- Then try to run the tool again to download the MSIX Packaging Tool driver. If you are still hitting issues, try changing your flight ring to Insider Fast, install the latest Windows updates and try again.

#### Installing MSIX Packaging Tool driver FOD in WSUS
Organizations that use Windows Server Update Services (WSUS) must take action to manually install the driver:
- [Check your version of Windows](https://support.microsoft.com/en-us/help/13443/windows-which-operating-system). You must be on at least Windows 10, version 1809. OS build 17763 
- Download the FOD .cab file for [Windows 10, version 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab)
- Individually-obtained Feature on Demand packages can be installed using [DISM command-line options](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). In an elevated PowerShell window type: Dism /Online /add-package /packagepath:(path) 

IT admins can also create [Side by side feature store (shared folder)](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275(v=ws.11)) to allow access to the MSIX Packaging tool driver FOD. You can find additional details at the bottom of this [post](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Otherwise if you have access to Enterprise or OEM channels you can obtain the driver from Windows 10 Features on Demand media from one of the following sources:
- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx) - VL access is required
- [OEM Portal](https://www.microsoftoem.com) - OEM access is required
- [MSDN Download](https://my.visualstudio.com/Downloads/Featured) - MSDN subscription is required

Individually-obtained Feature on Demand packages can be installed using DISM command-line options.

# Troubleshooting Conversions
## Reading the log files 
Whether your conversion was successful or not, log files are generated for every conversion. They can be found here: 
%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\

Failure codes are written and indicate any point of failure during the conversion process. The error codes are meant to be self explanatory. 

### Examples of failures during conversions 
#### Uninstallation Error - Exit Code 259
We noticed some installers exiting failing to convert with exit code 259. If you encountered this, it is because the installer spawns a thread and does not wait for it to complete. In other words, the main thread finishes installing but it exits with error 259 because the spawned a thread that is still running. We recommend that IT Pros use the appropriate install option for setup.exe. 

#### Internal warning messages
An example of warning signs you may encounter is this:
[Warning] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED
This indicates we detected COM registration that do not have a match with MSIX today. If you convert an app and it works, changes are these were unnecessary COM entries. However, if you see missing behavior than that means that this COM behavior was not captured during 
the conversion. 
Note: this may only occur in early verisons of the tool

### Please file feedback 
The best way for us to get your feedback is through the Feedback Hub. Here are the steps to file feedback 
1. Open 'Feedback Hub' app or hit Windows + F 
2. Provide a title and necessary steps to reproduce the issue 
3. Under Category, select Apps and select MSIX Packaging Tool 
4. Attach logs associated to the conversion. You can find the logs in the folder provided above. 
5. Attach the converted MSIX package (if possible) 
6. Click Submit 

#### Other known issues
- Restarting the machine during application installation is not supported. Please ignore the restart request if possible or pass an argument to the installer to not require a restart.
- Installers may require certain frameworks or drivers to be installed prior to installation. To look up frameworks and drivers on the machine that is being used to convert apps please use the following queries: driverquery /v | Out-File
or driverquery /v | Out-File "path to text file"



