---
title: Known issues and troubleshooting 
description: Describes known issues and provides troubleshooting tips for the MSIX Packaging Tool. 
author: dianmsft
ms.author: diahar
ms.date: 02/26/2019
ms.topic: article
keywords: msix packaging tool, known issues, troubleshooting
ms.localizationpriority: medium
ms.custom: RS5
---
# Known issues and troubleshooting

This article describes known issues and provides troubleshooting tips to consider when converting your apps to MSIX using the MSIX Packaging Tool.

## Known issues

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

## Other known issues

- Restarting the machine during application installation is not supported. Ignore the restart request if possible or pass an argument to the installer to not require a restart.
- Installers may require certain frameworks or drivers to be installed prior to installation. To look up frameworks and drivers on the machine that is being used to convert apps, use the following queries: ```driverquery /v | Out-File```
or ```driverquery /v | Out-File "path to text file"```

# Troubleshooting

## Log files

Whether or not your conversion was successful, log files are generated for every conversion. They can be found here: 
%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\

Failure codes are written and indicate any point of failure during the conversion process. The error codes are meant to be user friendly.

## Examples of failures during conversions

### Uninstallation Error - Exit Code 259

Some installers might fail to convert with exit code 259. This indicates that the installer spawned a thread and did not wait for it to complete. In other words, the main thread finished installing but it exited with error 259 because it spawned a thread that is still running. We recommend that you use the appropriate install option for setup.exe.

### Internal warning messages

You may encounter warning messages such as:
**[Warning] W_COM_PUBFAIL_INPROC_SERVER_NOT_SUPPORTED**.
This indicates that the MSIX Packaging Tool detected a COM registration that does not have a match with MSIX today. If you convert an app and it works, chances are these were unnecessary COM entries. However, if you see missing behavior, that means that this COM behavior was not captured during the conversion.

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
