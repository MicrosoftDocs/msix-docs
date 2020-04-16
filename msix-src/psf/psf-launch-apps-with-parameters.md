---
Description: The Package Support Framework can be used to lauch applications which require arguments passed to it..
title: Package Support Framework - Launch Apps with Parameters
ms.date: 03/30/2020
ms.topic: article
keywords: windows 10, uwp, msix, psf
ms.localizationpriority: medium
ms.custom: RS5
---

# Package Support Framework - Launch Apps with Parameters
The Package Support Framework uses a config.json file to configure the behavior of an application.

## Proceedure
To configure the application execution to receive parameter values passed into it during application launch, the following steps must be completed. 

1. Download the Package Support Framework
1. Configure the Package Support Framework (config.json)
1. Inject the Package Support Framework files into the Application package
1. Update the application's manifest file
1. Re-package the application

## Download the Package Support Framework
The Package Support Framework can be retrieved by using the standalone Nuget commandline tool or via Visual Studio.

### Nuget comandline tool:
Install the Nuget commandline tool from the nuget website: [https://www.nuget.org/downloads](https://www.nuget.org/downloads). After installing, run the following commandline in an administrative PowerShell window.

``` powershell
nuget install Microsoft.PackageSupportFramework
```

### Visual Studio:
In Visual Studio, right-click on your solution/project node and pick one of the Manage Nuget Packages commands. Search for **Microsoft.PackageSupportFramework** or **PSF** to find the package on nuget.org. Then, install it.


## Create the config.json file

To specify the arguements required to launch the application, you must modify the config.json file specifying the application executable, and parameters. Prior to configuring the config.json file, review the following table to understand the JSON strucuture.

### Json Schema

|Array	        | key	            | Value  |
|---------------|-------------------|--------|
| applications	| id        	    | Use the value of the ID attribute of the Application element in the package manifest.|
| applications	| executable	    | The package-relative path to the executable that you want to start. It's the value of the Executable attribute of the Application element. |
| applications	| arguments         | (Optional) Command line parameter arguments for the executable. |
| applications	| workingDirectory	| (Optional) A package-relative path to use as the working directory of the application that starts. If you don't set this value, the operating system uses the System32 directory as the application's working directory. If you supply a value in the form of an empty string, it will use the directory of the referenced executable. |
| applications	| monitor	        | (Optional) If present, the monitor identifies a secondary program that is to be launched prior to starting the primary application. |
| processes 	| executable        | In most cases, this will be the name of the executable configured above with the path and file extension removed. |
| fixups	    | dll   	        | Package-relative path to the fixup, .msix/.appx to load. |
| fixups	    | config	        | (Optional) Controls how the fixup dll will behave. The exact format of this value varies on a fixup-by-fixup basis as each fixup can interpret this "blob" as it wants.|

The applications, processes, and fixups keys are arrays. That means that you can use the config.json file to specify more than one application, process, and fixup DLL.

An entry in processes has a value named executable, the value for which should be formed as a RegEx string to match the name of an executable process without path or file extension. The launcher will expect Windows SDK std:: library RegEx ECMAList syntax for the RegEx string.

``` json
{
    "applications": [
      {
        "id": "PSFExampleID",
        "executable": "VFS\\ThisPCDesktopFolder\\IG-Bentley.exe",
        "workingDirectory": "",
      }
    ],
}
```

## Inject the Package Support Framework files to the Application package

### Create the package staging folder
If you have a .msix (or .appx) file already, you can unpack its contents into a layout folder that will serve as the staging area for your package. You can do this from a command prompt using the MakeAppx tool.

MakeAppx tool can be found in its default location:

| OS Architecture | Directory                                                   |
|-----------------|-------------------------------------------------------------|
| Windows 10 x86  | C:\Program Files\Windows Kits\10\bin\[**Version**]\x86\makeappx.exe       |
| Windows 10 x64  | C:\Program Files (x86)\Windows Kits\10\bin\[**Version**]\x64\makeappx.exe |

```powershell
makeappx unpack /p PrimaryApp.msix /d PackageContents
```

The above PowerShell command will export the contents of the application into a local directory.

### Inject required files
Add the required 32-bit and 64-bit Package Support Framework DLL(s) and executable files to the package directory. Use the following table as a guide. You will also want to include any runtime fixes as required. Visit [Package Support Framework Runtime fixes](https://docs.microsoft.com/windows/msix/psf/package-support-framework) Docs article for guidance on using the Package Support Framework for runtime fixes.

| Application executable is x64 | Application executable is x86     |
|-------------------------------|-----------------------------------|
| PSFLauncher64.exe             | PSFLauncher32.exe                 |
| PSFRuntime64.dll              | PSFLauncher32.dll                 |
| PSFRunDll64.exe               | PSFRunDll32.exe                   |

These above identified files, as well as the **config.json** file must be placed within the root of your application package directory.


## Update the application's manifest
Open the applications manifest in a text editor, then set the Executable attribute of the application element to the name of the PSFLauncher executable file. If you know the architecture of your target application, select the appropriate version, PSFLauncher[x64/x86].exe. If unknown, PSFLauncher32.exe will work in all cases.

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

## Re-package the application

Re-package the application using the makeappx tool. See the following example of how to package and sign the application using PowerShell:

``` powershell
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
signtool sign /a/v/fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```
