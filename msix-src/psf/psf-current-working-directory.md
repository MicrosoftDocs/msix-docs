---
description: Provides guidance on how to apply runtime working directory fix up with the Package Support Framework.
title: Package Support Framework - Working Directory fixup
ms.date: 12/16/2020
ms.topic: article
keywords: windows 10, uwp, psf, package support framework, working directory, workingdirectory, msix
ms.localizationpriority: medium
---

# Package Support Framework - Working Directory fixup

## Investigation

Windows Apps will redirect specific directories that are related to the application to the Windows App container folder. If an application creates a subfolder (`C:\Program Files\Vendor\subfolder`) as part of the installation, and later calls this subfolder, it will fail to find the directory as it does not exist.

Using the Package Support Framework (PSF), enhancements can be made to the Windows App package to resolve this issue. First, we must identify the failure, and directory paths that are being requested by the app.

#### Capture the Windows App Failure

Filtering the results is an optional step, that will make viewing application related failures easier. To do this, we will create two filter rules. The first an include filter for the application process name, and the second is an inclusion of any results that are not successful.

1. Download and extract the [SysInternals Process Monitor](/sysinternals/downloads/procmon) to the **C:\PSF\ProcessMonitor** directory.
1. Open Windows Explorer and navigate to the extracted SysInternals Process Monitor Folder
1. Double-click the SysInternals Process Monitor (procmon.exe) file, launching the app.
1. If prompted by UAC, select the **Yes** button.
1. In the Process Monitor Filter Window, select the first drop-down menu labeled with **Architecture**.
1. Select **Process Name** from the drop-down menu.
1. In the next drop-down menu, verify that it is set with the value of **is**.
1. In the text field type the process name of your App (Example: PSFSample.exe).
    :::image type="content" source="images/procmon-filterdialog-processname-psfsample.png" alt-text="Example of the Process Monitor Filter Windows with App Name":::
1. Select the **Add** button.
1. In the Process Monitor Filter Window, select the first drop-down menu labeled **Process Name**.
1. Select **Result** from the drop-down menu.
1. In the next drop-down menu, select it, and select **is not** from the drop-down menu.
1. In the text field type: **SUCCESS**.
    :::image type="content" source="images/procmon-filterdialog-result-success.png" alt-text="Example of the Process Monitor Filter Windows with Result":::
1. Select the **Add** button.
1. Select the **Ok** button.
1. launch the Windows App, trigger the error, and close the Windows App.

#### Review the Windows App Failure Logs

After capturing the Windows App processes, the results will need to be investigated to identify if the failure is related to the working directory.

1. Review the SysInternals Process Monitor results, searching for failures outlined in the above table.
1. If the results show an **"Name Not Found"** result, with the details **"Desired Access: ..."** for your specific app targeting a directory outside of the **"C:\Program Files\WindowsApps\\...\\"** (as seen in the below image), then you have successfully identified a failure related with the working directory, use the [PSF Support - Filesystem Access]() article for guidance on how to apply the PSF correction to your app.
:::image type="content" source="images/procmon-psfsampleapp-readfailure.png" alt-text="Displays the error message witnessed in the SysInternals Process Monitor for failure to write to directory.":::

## Resolution

Windows Apps will redirect specific directories that are related to the application to the Windows App container folder. If an application creates a subfolder (`C:\Program Files\Vendor\subfolder`) as part of the installation, and later calls this subfolder, it will fail to find the directory as it does not exist.

To resolve the issue related to the Windows App referencing an incorrect Working Directory, we must follow the following four steps:

1. [Stage the Windows App to a local directory](#stage-the-windows-app)
1. [Create the Config.json and inject required PSF Files](#create-and-inject-required-psf-files)
1. [Update the Windows App AppxManifest file](#update-appxmanifest) 
1. [Repackage and sign the Windows App](#re-package-the-application)

The above steps provide guidance through extracting the content of the Windows App to a local staged directory, injecting the PSF fixup files into the staged Windows App directory, configuring the Application Launcher to point to the PSF launcher, then configuring the PSF config.json file to redirect the PSF launcher to the app specifying the working directory.

### Download and Install Required Tools
This process will guide you through the retrieval of, and usage of the following tools:
- NuGet Client Tool
- Package Support Framework
- Windows 10 SDK (latest version)
- SysInternals Process Monitor

The following will provide step-by-step guidance on downloading and installing the required tools.

1. Download the latest (non-preview) version of the [NuGet client tool](https://www.nuget.org/downloads), and save the **nuget.exe** in the `C:\PSF\nuget` folder.
1. Download the Package Support Framework using [Nuget](/nuget/install-nuget-client-tools#nugetexe-cli) by running the following from an Administrative PowerShell window:
    ```PowerShell
    Set-Location "C:\PSF"
    .\nuget\nuget.exe install Microsoft.PackageSupportFramework
    ``` 
    
1. Download and install the [Windows 10 Software Development Toolkit (Win 10 SDK)](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Download the [Win 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).
    1. Run the **winsdksetup.exe** that was downloaded in the previous step.
    1. Select the **Next** button.
    1. Select only the following three features for install:
        - Windows SDK Signing Tools for Desktop Apps
        - Windows SDK for UWP C++ Apps
        - Windwos SDK for UWP Apps Localization
    1. Select the **Install** button.
    1. Select the **Ok** button.

### Stage the Windows App
By staging the Windows App, we will be extracting / unpackaging the contents of the Windows App to a local directory. Once the Windows App has been unpacked to the staging location, PSF fixup files can be injected correcting any unwanted experiences.

1. Open an Administrative PowerShell window.
1. Set the following variables targeting your specific app file, and Windows 10 SDK version:
    ```PowerShell
    $AppPath          = "C:\PSF\SourceApp\PSFSampleApp.msix"         ## Path to the MSIX App Installer
    $StagingFolder    = "C:\PSF\Staging\PSFSampleApp"                ## Path to where the MSIX App will be staged
    $OSArchitecture   = "x$((gwmi Win32_Processor).AddressWidth)"    ## Operating System Architecture
    $Win10SDKVersion  = "10.0.19041.0"                               ## Latest version of the Win10 SDK
    ```

1. Unpack the Windows App to the staging folder by running the following PowerShell cmdlet:
    ```PowerShell
    ## Sets the directory to the Windows 10 SDK
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    
    ## Unpackages the Windows App to the staging folder
    .\makeappx.exe unpack /p "$AppPath" /d "$StagingFolder"
    ```


### Create and inject required PSF Files
To apply corrective actions to the Windows App the a **config.json** file must be created, and supplied with information about the Windows App Launcher that is failing. If there are multiple Windows App Launchers that are experiencing issues, the **config.json** file can be updated with multiple entries.

After updating the **config.json** file, the **config.json** file and supporting PSF fixup files must then be moved into the root of the Windows App package. 


1. Open Visual Studio Code (VS Code), or any other text editor.
1. Create a new file, by selecting the **File** menu at the top of the VS Code, selecting **New File** from the drop-down menu.
1. Save the file as **config.json**, by select the **File** menu at the top of the VS Code window, selecting **Save** from the drop-down menu. In the Save As window, navigate to the Windows App Staging directory (**C:\PSF\Staging\PSFSampleApp**) and set the **File Name** as `config.json`. Select the **Save** button.
1. Copy the following code to the newly created **config.json** file.
    ```JSON
    {
        "applications": [
            {
                "id": "",
                "executable": "",
                "workingDirectory": ""
            }
        ],
        "processes": [
            {
                "executable": ""
            }
        ]
    }
    ```

1. Open the staged Windows App **AppxManifest** file located in the Windows App staging folder (**C:\PSF\Staging\PSFSampleApp\AppxManifest.xml**) using VS Code, or another text editor.
    ```xml
    <Applications>
        <Application Id="PSFSAMPLE" Executable="VFS\ProgramFilesX64\PS Sample App\PSFSample.exe" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements BackgroundColor="transparent" DisplayName="PSFSample" Square150x150Logo="Assets\StoreLogo.png" Square44x44Logo="Assets\StoreLogo.png" Description="PSFSample">
            <uap:DefaultTile Wide310x150Logo="Assets\StoreLogo.png" Square310x310Logo="Assets\StoreLogo.png" Square71x71Logo="Assets\StoreLogo.png" />
        </uap:VisualElements>
        </Application>
    </Applications>
    ```

1. Copy the value in the **ID** field located in the **AppxManifest.xml** file located in `Package.Applications.Application` to the Applications ID field in the **config.json** file.
    :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Image circling the location of the ID within the AppxManifest file.":::

1. Copy the package-relative path from the **Executable** field located in the **AppxManifest.xml** file located in `Package.Applications.Application` to the Applications **Executable** field in the **config.json** file.
    :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Image circling the location of the executable within the AppxManifest file.":::

1. Copy the package-relative parent path from the **Executable** field located in the **AppxManifest.xml** file located in `Package.Applications.Application` to the Applications **WorkingDirectory** field in the **config.json** file.
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image circling the location of the working directory within the AppxManifest file.":::

1. Copy the executable name from the **Executable** field located in the **AppxManifest.xml** file located in `Package.Applications.Application` to the Processes **Executable** field in the **config.json** file.
    :::image type="content" source="images/appxmanifest-application-processexecutable.png" alt-text="Image circling the location of the process executable within the AppxManifest file.":::

1. Save the updated **config.json** file.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe",
            "workingDirectory": "VFS/ProgramFilesX64/PS Sample App/"
            }
        ],
        "processes": [
            {
            "executable": "PSFSample"
            }
        ]
    }
    ```

1. Copy the following three files from the Package Support Framework based on the application executable architecture to the root of the staged Windows App. The following files are located within the **.\Microsoft.PackageSupportFramework.<Version>\bin**.

    | Application (x64) | Application (x86) | 
    |-------------------|-------------------|
    | PSFLauncher64.exe | PSFLauncher32.exe |
    | PSFRuntime64.dll  | PSFRuntime32.dll  |
    | PSFRunDll64.exe   | PSFRunDll32.exe   |


### Update AppxManifest
After creating and updating the **config.json** file, the Windows App's **AppxManifest.xml** must be updated for each Windows App Launcher that was included in the **config.json**. The **AppxManifest**'s Applications must now target the **PSFLauncher.exe** associated with the applications architecture.


1. Open File Explorer, and navigate to the Staged MSIX App folder (**C:\PSF\Staging\PSFSampleApp**).
1. Right-click on **AppxManifest.xml**, and select **Open with Code** from the drop-down menu (Optionally, you can open with another text editor).

1. Update the AppxManifest.xml file with the following information:
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


### Re-package the application
All of the corrections have been applied, now the Windows App can be re-packaged into an MSIX and signed using a code signing certificate.

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

1. Repack the Windows App from the staging folder by running the following PowerShell cmdlet:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\makeappx.exe pack /p "$AppPath" /d "$StagingFolder"
    ```

1. Sign the Windows App by running the following PowerShell cmdlet:
    ```PowerShell
    Set-Location "${env:ProgramFiles(x86)}\Windows Kits\10\Bin\$Win10SDKVersion\$OSArchitecture"
    .\signtool.exe sign /v /fd sha256 /f $CodeSigningCert /p $CodeSigningPass $AppPath
    ```