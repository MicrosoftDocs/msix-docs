---
description: Provides guidance on how to apply runtime fixes with the Package Support Framework.
title: Package Support Framework - Launching Windows Apps with Parameters
ms.date: 12/16/2020
ms.topic: article
keywords: windows 10, uwp, psf, package support framework, arguments, app launcher, parameters, shortcut, msix
ms.localizationpriority: medium
---


# Launching Windows App with Parameters

## Investigation

Some Windows App launchers in the start menu require the use of parameters to be passed to the executable when launching the Windows App. To accomplish this, we will first need to identify the launcher requiring the parameter before integrating the Windows App with the Package Support Framework.

#### Identifying Windows App Launcher Parameter Requirement

1. Install your Windows App on a test machine.
1. Open the **Windows Start Menu**.
1. Locate and select your Windows App Launcher from within the Start Menu.
1. If the App launches, then you do not have any issues (test all associated Windows App Launchers in the Start Menu).
1. Uninstall the Windows App from the test machine.
1. Using the Win32 installation media, install the application to your test machine.
1. Open the **Windows Start Menu**.
1. Locate and right-click on your Windows App from within the Start Menu.
1. Select **More** >> **Open File Location** from within the drop-down menu.
1. Right-click on the first associated application shortcut (repeat the next three steps for all associated application shortcuts).
1. Select **Properties** from the drop-down menu.
1. Review the value in the text box right of **Target**. After the application file path, if there is a parameter listed, this app
    :::image type="content" source="images/contosoapp-fileproperties-target-parameter.png" alt-text="Example of the File Property Window with Parameter in Target":::

1. Record the parameter value for future use.


## Resolution

Windows Apps will redirect specific directories that are related to the application to the Windows App container folder. If an application attempts to write to the Windows App container, an error will trigger, and the write will fail. 

To resolve the issue related to the Windows App failing to write to the Windows App container, we must follow the following four steps:

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
                "arguments": ""
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

1. Set the `applications.id` value in the *config.json* to be the same value as found in the **Applications.Application.ID** field of the *AppxManifest.xml* file.
    :::image type="content" source="images/appxmanifest-application-id.png" alt-text="Image circling the location of the ID within the AppxManifest file.":::

1. Set the `applications.executable` value in the *config.json* to target the relative path to the application located in **Applications.Application.Executable** field of the *AppxManifest.xml* file.
    :::image type="content" source="images/appxmanifest-application-executable.png" alt-text="Image circling the location of the executable within the AppxManifest file.":::

1. Set the `applications.arguments` value in the *config.json* to match with the argument used to launch the application. See recorded value from the final step of the [Investigation - Identifying Windows App Launcher Parameter Requirement](#identifying-windows-app-launcher-parameter-requirement) guidance.

1. Set the `applications.workingdirectory` value in the *config.json* to target the relative folder path found in the **Applications.Application.Executable** field of the *AppxManifest.xml* file.
    :::image type="content" source="images/appxmanifest-application-workingdirectory.png" alt-text="Image circling the location of the working directory within the AppxManifest file.":::

1. Save the updated **config.json** file.
    ```json
    {
        "applications": [
            {
            "id": "PSFSample",
            "executable": "VFS/ProgramFilesX64/PS Sample App/PSFSample.exe",
            "arguments": "/bootfromsettingshortcut"
            }
        ]
    }
    ```

1. Copy the following four files from the Package Support Framework based on the application executable architecture to the root of the staged Windows App. The following files are located within the **.\Microsoft.PackageSupportFramework.\<Version\>\bin**.

    | Application (x64)          | Application (x86)          | 
    |----------------------------|----------------------------|
    | PSFLauncher64.exe          | PSFLauncher32.exe          |
    | PSFRuntime64.dll           | PSFRuntime32.dll           |
    | PSFRunDll64.exe            | PSFRunDll32.exe            |


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