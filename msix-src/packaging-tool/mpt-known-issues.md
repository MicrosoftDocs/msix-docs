### Known Issues
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
- Download the FOD .cab file for [Windows 10, version 1809]()
- Individually-obtained Feature on Demand packages can be installed using [DISM command-line options](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options). In an elevated PowerShell window type: Dism /Online /add-package /packagepath:(path) 

IT admins can also create [Side by side feature store (shared folder)](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275(v=ws.11)) to allow access to the MSIX Packaging tool driver FOD. You can find additional details at the bottom of this [post](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Language-pack-acquisition-and-retention-for-enterprise-devices/ba-p/275404).

Otherwise if you have access to Enterprise or OEM channels you can obtain the driver from Windows 10 Features on Demand media from one of the following sources:
- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx) - VL access is required
- [OEM Portal](https://www.microsoftoem.com) - OEM access is required
- [MSDN Download](https://my.visualstudio.com/Downloads/Featured) - MSDN subscription is required
Individually-obtained Feature on Demand packages can be installed using DISM command-line options.

#### Other known issues
- Restarting the machine during application installation is not supported. Please ignore the restart request if possible or pass an argument to the installer to not require a restart.
- Setting EnforceMicrosoftStoreVersioningRequirements=true, when using the command line interface, will throw an error, even if the vesrion is set correctly. To work around this issue, use EnforceMicrosoftStoreVersioningRequirements=false in the conversion template file.
- Adding files to MSIX packages in package editor does not add the file to the folder that the user right-clicks. To work around this issue, ensure that the file being added is in the correct classic app location. For example if you want to add a file in the VFS\ProgramFilesx86\MyApp folder, copy the file locally to your C:\Program Files (86)\MyApp location first, then in the package editor right-click Package files, and then click Add file. Browse to the newly copied file, then click Save.

### MSIX Packaging Tool logs
Conversion logs can be found at %localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\
