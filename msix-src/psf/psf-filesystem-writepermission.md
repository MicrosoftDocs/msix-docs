---
title: Package Support Framework - Filesystem Write Permission fixup
description: Provides guidance on how to apply Filesystem Write permission fixes with the Package Support Framework.
ms.date: 05/10/2022
ms.topic: article
keywords: windows 10, uwp, psf, package support framework, filesystem, write permission, msix
ms.custom: kr2b-contr-experiment
---

# How to fix Package Support Framework - Filesystem Write Permission error

This article describes how to use the Package Support Framework (PSF) to resolve a Package Support Framework - Filesystem Write Permission error.

Windows Apps redirects specific directories that are related to the application to the Windows App container folder. If an application attempts to write to the Windows App container, an error triggers, and the write fails. You can make enhancements to the Windows App package to resolve this issue.

## Investigation

First, identify the failure, and the directory paths that the app is requesting.

#### Capture the Windows App failure

Filtering the results is optional, but makes it easier to see application-related failures. To filter results, you create two filter rules. The first filter includes the application process name, and the second filter includes any results that aren't successful.

1. Download and extract the [SysInternals Process Monitor](/sysinternals/downloads/procmon) to the *C:\\PSF\\ProcessMonitor* directory.
1. Open Windows Explorer and navigate to the extracted SysInternals *ProcessMonitor* folder.
1. Select the SysInternals Process Monitor *procmon.exe* file to launch the app.
1. If prompted by UAC, select **Yes**.
1. In the **Process Monitor Filter** window, select **Process Name** from the first field's dropdown menu.
1. Verify that **is** appears in the next field.
1. In the next field, enter your app's process name, for example *PSFSample.exe*.

   :::image type="content" source="images/procmon-filterdialog-processname-psfsample.png" alt-text="Example of the Process Monitor Filter window with app name.":::

1. Select **Add**.
1. In the **Process Monitor Filter** window, select **Result** from the first field's dropdown menu.
1. In the next field, select **is not** from the dropdown menu.
1. In the text field, enter *SUCCESS*.

   :::image type="content" source="images/procmon-filterdialog-result-success.png" alt-text="Example of the Process Monitor Filter window with Result.":::

1. Select **Add**, and then select **OK**.
1. Launch the Windows app, trigger the error, and then close the Windows app.

#### Review the Windows App failure logs

After you capture the Windows app processes, investigate the results to determine whether the failure is related to the working directory.

Review the SysInternals Process Monitor failure results. If the results include an **Access denied** result, with the details **Desired Access: ...** for your app targeting **C:\Program Files\WindowsApps\\...\\** as in the following screenshot, you've identified a failure related to the working directory.

:::image type="content" source="images/procmon-psfsampleapp-writefailure.png" alt-text="Displays the error message witnessed in the SysInternals Process Monitor for failure to write to directory.":::

If you identify this error, apply the following PSF correction to your app.
 
 ## Resolution

To resolve the issue of the Windows app failing to write to the Windows app container, follow these steps:

1. Extract the content of the Windows app to a [local staged directory](#stage-the-windows-app).
1. [Create the config.json and inject the PSF fixup files](#create-and-inject-required-psf-files) into the staged Windows app directory.
1. Configure the application launcher to point to the PSF launcher, and configure the PSF *config.json* file to redirect the PSF launcher to the app, specifying the working directory.
1. [Update the Windows app AppxManifest file](#update-appxmanifest).
1. [Repackage and sign the Windows app](#re-package-the-application).

### Download and install required tools

This process requires the following tools:

- NuGet client tool
- Package Support Framework (PSF)
- Windows 10 Software Development Toolkit (Win 10 SDK), latest version
- SysInternals Process Monitor

To download and install NuGet and PSF:

1. Download the latest non-preview version of the [NuGet client tool](https://www.nuget.org/downloads), and save *nuget.exe* in *C:\\PSF\\nuget*.

1. Download and install the Package Support Framework using [Nuget](/nuget/install-nuget-client-tools#nugetexe-cli) by running the following command from an administrative PowerShell window:
   ```powershell
   Set-Location "C:\PSF"
   .\nuget\nuget.exe install Microsoft.PackageSupportFramework
   ``` 

To download and install the Windows 10 SDK:

1. Download the [Win 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
1. Run *winsdksetup.exe* .
1. Select **Next**.
1. Select only the following three features:
   - Windows SDK Signing Tools for Desktop Apps
   - Windows SDK for UWP C++ Apps
   - Windwos SDK for UWP Apps Localization
1. Select **Install**, and then select **OK**.

### Stage the Windows app

Staging the Windows app extracts and unpackages the contents of the Windows app to a local directory. Once the Windows App is unpacked to the staging location, you can inject PSF fixup files to correct any unwanted experiences.

1. In an administrative PowerShell window, set the following variables to target your specific app file and Windows 10 SDK version:
   ```powershell
   $AppPath          = "C:\PSF\SourceApp\PSFSampleApp.msix"         ## Path to the MSIX App Installer
   $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
   $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
   $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
   ```

1. Unpack the Windows app to the staging folder by running the following PowerShell cmdlet:
   ```powershell
   ## Sets the directory to the Windows 10 SDK
   Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
   
   ## Unpackages the Windows App to the staging folder
   .\makeappx.exe unpack /p "$AppPath" /d "$StagingFolder"
   ```

### Create and inject required PSF files

To correct the Windows app, you create a *config.json* file with information about the Windows app launcher that fails. If multiple Windows app launchers are experiencing issues, you can configure the *config.json* file with multiple entries.

After you create the *config.json* file, you move the *config.json* and supporting PSF fixup files into the root of the Windows app package.

1. Open Visual Studio Code or another text editor.
1. Create a new file named *config.json* in the Windows app staging directory, *C:\\PSF\\Staging\\PSFSampleApp*.
1. Copy the following code to the newly created *config.json* file.
    ```json
    {
        "applications": [
            {
                "id": "",
                "executable": ""
            }
        ],
        "processes": [
            {
                "executable": "",
                "fixups": [
                {
                    "dll": "",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "",
                                    "patterns": [
                                        ""
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
            }
        ]
    }
    ```

1. Open the *AppxManifest.xml* file in the Windows app staging folder. *AppxManifest.xml* looks something like the following example:
    ```xml
    <Applications>
        <Application Id="PSFSAMPLE" Executable="VFS\ProgramFilesX64\PS Sample App\PSFSample.exe" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements BackgroundColor="transparent" DisplayName="PSFSample" Square150x150Logo="Assets\StoreLogo.png" Square44x44Logo="Assets\StoreLogo.png" Description="PSFSample">
            <uap:DefaultTile Wide310x150Logo="Assets\StoreLogo.png" Square310x310Logo="Assets\StoreLogo.png" Square71x71Logo="Assets\StoreLogo.png" />
        </uap:VisualElements>
        </Application>
    </Applications>
    ```

1. Make the following changes in the *config.json* file:

   - Set the `applications.id` value to be the same value as in the `Applications.Application.ID` field of the *AppxManifest.xml* file.
   
     :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Image showing the location of the ID within the AppxManifest file.":::

   - Set the `applications.executable` value to target the relative path to the application located in the `Applications.Application.Executable` field of the *AppxManifest.xml* file.

     :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Image showing the location of the executable within the *AppxManifest.xml* file.":::

   - Set the `applications.workingdirectory` value to target the relative folder path in the `Applications.Application.Executable` field of the *AppxManifest.xml* file.

     :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image showing the location of the working directory within the AppxManifest file.":::

   - Set the `process.executable` value to target the file name, without path and extension, in the `Applications.Application.Executable` field of the *AppxManifest.xml* file.

     :::image type="content" source="images/appxmanifest-application-processexecutable.png" alt-text="Image showing the location of the process executable within the AppxManifest file.":::

   - Set the `processes.fixups.dll` value to target the architecture-specific `FileRedirectionFixup.dll`. If the correction is for x64 architecture, set the value to be `FileRedirectionFixup64.dll`. If the architecture is x86, or is unknown, set the value to be `FileRedirectionFixup86.dll`.

   - Set the `processes.fixups.config.redirectedPaths.packageRelative.base` value to the package-relative folder path in the `Applications.Application.Executable` field of the *AppxManifest.xml* file.

     :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image showing the location of the working directory within the AppxManifest file.":::

   - Set the `processes.fixups.config.redirectedPaths.packageRelative.patterns` value to match the file type the application creates. If you use `.*\\.log`, the PSF redirects all log file writes in the `processes.fixups.config.redirectedPaths.packageRelative.base` directory and child directories.

1. Save the updated *config.json* file. The updated file looks something like the following example:
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe"
            }
        ],
        "processes": [
            {
                "executable": "PSFSample",
                "fixups": [
                    {
                        "dll": "FileRedirectionFixup64.dll",
                        "config": {
                            "redirectedPaths": {
                                "packageRelative": [
                                    {
                                        "base": "VFS/ProgramFilesX64/PS Sample App/",
                                        "patterns": [
                                            ".*\\.log"
                                        ]
                                    }
                                ]
                            }
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Copy the following files from the Package Support Framework for the application executable architecture to the root of the staged Windows app. You can find the files in *.\\Microsoft.PackageSupportFramework.\\\<Version>\\bin*.

    | Application (x64)          | Application (x86)          | 
    |----------------------------|----------------------------|
    | *PSFLauncher64.exe*          | *PSFLauncher32.exe*          |
    | *PSFRuntime64.dll*           | *PSFRuntime32.dll*           |
    | *PSFRunDll64.exe*            | *PSFRunDll32.exe*            |
    | *FileRedirectionFixup64.dll* | *FileRedirectionFixup64.dll* |


### Update AppxManifest

After you create and update the *config.json* file, update the Windows app's *AppxManifest.xml* file for each Windows app launcher you included in the *config.json*. The *AppxManifest.xml* `Applications` must now target the *PSFLauncher.exe* associated with the application architecture.

1. Open *AppxManifest.xml* in the Staged MSIX App folder, *C:\\PSF\\Staging\\PSFSampleApp*.
1. Update *AppxManifest.xml* with the following code:
    ```xml
    <Package ...>
    ...
    <Applications>
        <Application Id="PSFSample"
                    Executable="PSFLauncher32.exe"
                    EntryPoint="Windows.FullTrustApplication">
        ...
        </Application>
    </Applications>
    </Package>
    ```

### Repackage the application

Now that you applied all of the corrections, repackage the Windows app into an MSIX and sign it with a code signing certificate.

1. Open an Administrative PowerShell Window.
1. Set the following variables:
   ```PowerShell
   $AppPath          = "C:\PSF\SourceApp\PSFSampleApp_Updated.msix" ## Path to the MSIX App Installer
   $CodeSigningCert  = "C:\PSF\Cert\CodeSigningCertificate.pfx"     ## Path to your code signing certificate
   $CodeSigningPass  = "<Password>"                                 ## Password used by the code signing certificate
   $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
   $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
   $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
   ```

1. Repack the Windows app from the staging folder by running the following PowerShell cmdlet:
   ```PowerShell
   Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
   .\makeappx.exe pack /p "$AppPath" /d "$StagingFolder"
   ```

1. Sign the Windows app by running the following PowerShell cmdlet:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\signtool.exe sign /v /fd sha256 /f $CodeSigningCert /p $CodeSigningPass $AppPath
    ```
