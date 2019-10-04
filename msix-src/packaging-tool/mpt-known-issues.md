---
title: MSIX Packaging Tool Known Issues and Troubleshooting Tips
description: Describes known issues and provides troubleshooting tips for the MSIX Packaging Tool. 
ms.date: 02/26/2019
ms.topic: article
keywords: msix packaging tool, known issues, troubleshooting
ms.localizationpriority: medium
ms.custom: RS5
---
# MSIX Packaging Tool Known Issues and Troubleshooting Tips

This article describes known issues and provides troubleshooting tips to consider when converting your apps to MSIX using the MSIX Packaging Tool.

## MSIX Packaging Tool Known Issues
### Acquiring the MSIX Packaging Tool - Insider Flight
If you have opted into our flighting group, make sure you have the correct version of the MSIX Packaging Tool. To do this, please do the following 
- Release notes – go to our Release Notes page to double check that the MSIX Packaging Tool app you have installed is the latest version that is available. 
- Manually update through Microsoft Store app. If this option if available to you open the Microsoft Store app. Go to Downloads and Updates and click Get updates. 
- Acquiring MSIX Packaging Tool offline – please follow the instructions highlighted on [this page]( https://docs.microsoft.com/en-us/windows/msix/packaging-tool/mpt-known-issues#getting-the-msix-packaging-tool-for-offline-use) to ensure you get the latest app through our offline process. 
- Check whether you are in WSUS – see below 
- Check the [Release Notes]( https://docs.microsoft.com/en-us/windows/msix/packaging-tool/release-notes/history) to check the latest Release 

If you are interested in joining our Insider Program, click [here](https://aka.ms/MSIXPackagingPreviewProgram) for more information.

### MSIX Packaging Tool driver considerations

MSIX Packaging Tool driver is delivered as a [Feature on Demand (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) package from Windows Update and will fail to install if the Windows Update service is disabled on the machine or Windows Insider flight ring settings do no match the OS build of the machine.

### Installing MSIX Packaging Tool driver FOD on Windows Insider builds

If you are having issues installing the driver on a Windows 10 Insider Preview build:

- Navigate to **Settings** > **Updates & Security** > **Windows Insider Program**.
- If you see a message that Insider Preview build settings need attention, click on the **Fix me** button to log in again. You might have to go to Windows Update page and check for update before settings change takes effect.
- Try to run the tool again to download the MSIX Packaging Tool driver. If you are still encountering issues, try changing your flight ring to Insider Fast, install the latest Windows updates and try again.

### Installing MSIX Packaging Tool driver FOD in WSUS

Organizations that use Windows Server Update Services (WSUS) must take action to manually install the driver:

- [Check your version of Windows](https://support.microsoft.com/help/13443/windows-which-operating-system). You must be on at least Windows 10, version 1809 (build 17763).
- Download the FOD .cab file for [Windows 10, version 1809](https://download.microsoft.com/download/8/4/3/8436215A-42DB-4FD2-966D-60D436D6EEFC/Msix-PackagingTool-Driver-Package~31bf3856ad364e35~amd64~~.cab).
- Individually-obtained Feature on Demand (FOD) packages can be installed using [DISM command-line options](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). In an elevated PowerShell window type: ```Dism /Online /add-package /packagepath:(path)```

IT admins can also create [Side by side feature store (shared folder)](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) to allow access to the MSIX Packaging tool driver FOD. You can find additional details at the bottom of this [blog post](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Otherwise, if you have access to enterprise or OEM channels you can obtain the driver from Windows 10 Features on Demand media from one of the following sources:

- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx): Volume License access is required.
- [OEM Portal](https://www.microsoftoem.com): OEM access is required.
- [MSDN Download](https://my.visualstudio.com/Downloads/Featured): MSDN subscription is required.

Individually-obtained Feature on Demand packages can be installed using DISM command-line options.

### Getting the MSIX Packaging Tool for offline use

The MSIX Packaging Tool can be downloaded for offline use in the enterprise from the Microsoft Store for Business [web portal](https://businessstore.microsoft.com/store). You can learn more about offline distribution [here](https://docs.microsoft.com/microsoft-store/distribute-offline-apps). If you are encountering issues with the offline copy of the packaging tool, make sure you have the [offline copy of the license](https://docs.microsoft.com/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app) for the tool. 

After you have the offline version of the application, you can use [PowerShell](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps) to add the app package and license to your machine.

## Frameworks and Drivers
If the app requires a framework, make sure the framework is installed during the monitoring phase of the conversion. Go through the logs to ensure this is happening. If your app requires a driver to install, you need to evaluate whether this is absolutely required for your app to run properly. MSIX currently does not support driver installation. 

## Issues that may come up during conversion
- During conversion, installers may run services. Services are not captured during conversion. As a result, your app may install but it may run with issues.
- Some installers might fail to convert with exit code 259. This indicates that the installer spawned a thread and did not wait for it to complete. In other words, the main thread finished installing but it exited with error 259 because it spawned a thread that is still running. We recommend that you use the appropriate install option for setup.exe. 

## Issue with Signing
### Bad PE certificate (0x800700C1) 
This occurs when the package contains binary that has a corrupt certificate. To resolve this issue, use a dumpbin.exe /headers to dump the file headers and inspect for bad elements. Manually rewrite the headers to fix the issue. In general, MSIX Packaging tool automatically detects bad headers. If this issue persists please file feedback. More information can be found [here](https://docs.microsoft.com/en-us/windows/msix/desktop/desktop-to-uwp-known-issues#bad-pe-certificate-0x800700c1).

### Device Guard Signing 
Make sure to follow [these steps](https://docs.microsoft.com/en-us/windows/msix/package/signing-package-device-guard-signing). The key thing here is to make sure you are assigning the appropriate roles. 

### Expire Certification 
- Use a timestamp when you sign your package 
- You can resign with a valid sign or timestamp certification 

You can resign your app using the [Bach Conversion script](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts). 

# Troubleshooting

## Log files

Whether or not your conversion was successful, log files are generated for every conversion. They can be found here: 

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

Failure codes are written and indicate any point of failure during the conversion process. The error codes are meant to be user friendly.

### Log files from remote devices or VMs

If the conversion is performed on a remote device or a VM, we recommend that you copy the log files from that device and attach them as part of the feedback item. This will help us diagnose and resolve issues more efficiently. 

You will find the logs from the remote conversions here:
`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

It would even more beneficial if you can share the whole Logs folder that will include the operations occuring on the local client as well the remote server.

## Common Problems



## Sending feedback

The best way to send your feedback is through the **Feedback Hub**.
1. Open **Feedback Hub** or type **Windows + F**.
2. Provide a title and necessary steps to reproduce the issue.
3. Under **Category**, select **Apps** and select **MSIX Packaging Tool**.
4. Attach any [log files](#log-files) associated to the conversion. You can find the logs in the folder provided above.
5. Attach the converted MSIX package (if possible).
6. Click **Submit**.

You can also send us feedback directly from the MSIX Packaging Tool by going to the **Feedback** tab under **Settings**. 

> [!NOTE]
> It may take 24 hours for your feedback to get to us. Therefore if you are using a VM to convert your package, you may want to keep your VM on and in its current state for 24 hours after conversion. 
