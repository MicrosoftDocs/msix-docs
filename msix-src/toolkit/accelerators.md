---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       Accelerators # Add a title for the browser tab
description: Guide for developers to create accelerators # Add a meaningful description for search results
author:      fiza-microsoft # GitHub alias
ms.author:   fizaazmi # Microsoft alias
ms.topic:    conceptual # Add the ms.topic value
ms.date:     05/02/2023
---

# Accelerators

An Accelerator provides an efficient way to capture important information regarding conversion of legacy applications to MSIX format. It consists of important information regarding the package (application), the operating system on which conversion happens, as well as the steps required to fix the package for proper functioning of converted MSIX.

## Prerequisites

1.	Join the The MSIX Packaging Tool Insider Program  to get early access Preview build to try out accelerators

## Create an Accelerator 
1. Check out the sample accelerator here at [MSIX-Labs DeveloperLabs](https://github.com/microsoft/MSIX-Labs/tree/master/DeveloperLabs/SampleAccelerators) to see the accelerator structure, and use it to build your own accelerator.

## Definitions

- _PackageName_ : Package is an application or program (Win32, WPF or Windows Forms application) having a legacy (exe, msi etc) installer which is being converted to MSIX format.
- _PackageVersion_ : Package versions are associated with a specific release. In some cases you will see a perfectly formed [semantic](https://semver.org) version number, and in other cases you might see something different. These may be date driven, or they might have other characters with some package specific meaning.
-  _PublisherName_ : Name of the **original** publisher of the package.
- _EligibleForConversion_ : Some apps are prohibited for conversion due to security reasons, use of drivers etc. Hence, this flag is used to determine the eligibility for conversion. Accepted values can be found [here](#accepted-values-for-eligibleforconversion).
- _ConversionStatus_ : Determine status of application conversion. Accepted values can be found [here](#accepted-values-for-conversionstatus).
- _RemediationApproach_ : 
    - _SequenceNumber_ : Determines the sequence number of a fix-step. Fix steps to successfully convert the app need to be provided sequentially.
    - _Issue_ :
        - _Description_ : Text description of the actual issue encountered while conversion. For example, Registry errors or FileCreate errors in Procmon.
        - _Reference_ :  (Optional Field) link to the document containing detailed information about the issue.
    - _Fix_ :
        - _FixType_ : The specific kind of step. Example - If FixType is “Capability”, a specific capability needs to be added at this point. Accepted values can be found [here](#accepted-values-for-fixtype).
        - _Reference_ : Reference link to the document containing detailed information about the fix and how it needs to be done. This field is optional.
        - _FixDetails_ : To determine specific kind of fix required under a particular FixType. Example - If Fixtype is “Dependency”, then FixDetails would have a array-type field called "Dependencies" to list down all the dependencies that need to be added for the application. Use cases can be found [here](#use-cases-for-fixdetails).
        

- _MinimumPSFVersion_ : (Required only when atleast one of the FixType uses PSF or [PackageSupportFramework](https://docs.microsoft.com/en-us/windows/msix/psf/package-support-framework-overview)). Since PSF releases are backward-compatible, any version greater than this specified version will work.
- _AdditionalComments_ : To list additional information regarding app conversion, solely meant for human reading. This field is optional.
- _Edition_ : Edition of the Operating system. Example - Windows 10 Enterprise.
- _MinimumOSVersion_ :	Version of the Operating system. Example - 21H1. This field is to signify that any version greater than this specified OS version will work.
- _MinimumOSBuild_ : Build Version of the Operating System. Example - "19043.1165". This field is to signify that that any OS build greater than this specified OS build will work.
- _Architecture_ : Architecture of the package (application). (32/64 bit)
- _MSIXConversionToolVersion_ :	Version of MSIX Packaging Tool used for conversion. Example - 1.2021.709.0;
- _AcceleratorVersion_: Version of the accelerator being used. Currently the latest version is 1.0.0.

## Use Cases For ConversionStatus
- Successful - No Fix Required          
```yaml
ConversionStatus : Successful - No Fix Required
```

-  Successful - Fix Required   
```yaml
ConversionStatus: Successful - Fix Required

RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: App unable to install visual c++ dependency
    Fix:
      FixType: Dependency
      FixDetails:
        Dependencies:
          - Visual C++
```

-  Converted With Issues                
```yaml
ConversionStatus: Converted With Issues

RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Shortcut not captured
    Fix:
      FixType: EntryPoint 
      FixDetails:
        EntryPointIssue: ShortcutNotCaptured
        Solution:
          - Launch from start menu
```
-  Failed       
```yaml
ConversionStatus: Failed

RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Registry errors in Procmon
```

-  Not Eligible    
```yaml
EligibleForConversion: No - Driver Required

ConversionStatus: Not Eligible
```
## Use Cases For FixDetails
 - FixType: Capability
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Admin Access needed to run an app 
    Fix:
      FixType: Capability
      Reference: https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#:~:text=or%20Visual%20Studio.-,Elevation,-The%20allowElevation%20restricted
      FixDetails:
        Capabilities:
          - allowElevation
      
```
- FixType: Dependency
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: The app needs a 2008 C++ to be installed in the system
      Reference: https://forums.guru3d.com/threads/problem-running-afterburner.408768/
    Fix:
      FixType: Dependency
      Reference: https://forums.guru3d.com/threads/problem-running-afterburner.408768/
      FixDetails:
        Dependencies:
          - C++ 2008 runtime 
```
- FixType: InstallationPath
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Required permissions were not granted to the VFS folder and launcher.exe was not available during msix launch
    Fix:
      FixType: InstallationPath
      Reference: https://docs.microsoft.com/en-us/windows/msix/packaging-tool/create-app-package#package-information
      FixDetails:
        Path: C:/Users/User/AppData/Local
```
- FixType: Custom
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description:  Chromium is downloaded as zip (not exe or msi).
    Fix:
      FixType: Custom
      FixDetails:
        Solution:
          - MSIX Packaging Tool Installation Step, Unzip the chromium.zip and then launch chrome.exe.
```
- FixType: PSF
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: There were create file errors in process monitor
    Fix:
      FixType: PSF
      Reference: https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup
      FixDetails:
        PSFConfig:
          applications:
            - id: LINELAUNCHER
              executable: LINE/bin/LineLauncher.exe
              workingDirectory: LINE/bin/
          processes:
            - executable: LineLauncher
              fixups:
                - dll: FileRedirectionFixup.dll
                  config:
                    redirectedPaths:
                      packageRelative:
                        - base: LINE/Data/
                          patterns:
                            - .*\.tst
                        - base: LINE/bin/
                          patterns:
                            - .*
```
- FixType: Services
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: MSIX Packaging Tool failed to convert to MSIX stating a service is running outside the package.
    Fix:
      FixType: Services
      FixDetails:
        Exclude:
          - CleanupPSvc
```
- FixType: EntryPoint
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Shortcut not captured
      Reference: https://microsoft.visualstudio.com/DefaultCollection/OS/_workitems/edit/35877020
    Fix:
      FixType: EntryPoint 
      FixDetails:
        EntryPointIssue: ShortcutNotCaptured
        Solution:
          - Launch from start menu
```
- FixType: InstalledLocationVirtualization
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: Test Issue
    Fix:
      FixType: InstalledLocationVirtualization
      Reference: https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-installedlocationvirtualization
      FixDetails:
        UpdateActionsAttributes:
          ModifiedItems: keep
          DeletedItems: reset
          AddedItems: keep
```
- FixType: LoaderSearchPathOverride
```yaml
RemediationApproach:
  - SequenceNumber: 1
    Issue:
      Description: DLL not found 
    Fix:
      FixType: LoaderSearchPathOverride
      Reference: https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride
      FixDetails:
        FolderPaths: 
          - VFS\ProgramFilesX64\LINE\lib
          - VFS\ProgramFilesX64\LINE\bin
```

### Accepted Values for EligibleForConversion

- Yes                                       
- No                                   
- No - Driver Required                           

### Accepted Values For ConversionStatus

- Successful - No Fix Required          
- Successful - Fix Required             
- Converted With Issues               
- Failed                                
- Not Eligible

### Relation between EligibleForConversion and ConversionStatus

| EligibleForConversion        | ConversionStatus |
| ---------------------------- | ---------------- |
| Yes                          | Successful - No Fix Required, Successful - Fix Required, Converted With Issues |             
| No                           | Failed, Not Eligible |
| No - Driver Required         | Not Eligible |

### Accepted Values for FixType

| Accepted Values | Definitions |
| ------------------- | ---------------- |
| Capability | Capabilities needed (eg: allowElevation, uiAccess etc.) for MSIX application to work. To be added in AppManifest or via Capabilities page in MSIX packaging tool during conversion. See [here](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-capabilities) for more details. |
| Dependency | Dependencies needed (eg: C++ 2008 Redistributable x86) for MSIX application to work. To be downloaded externally in the OS environment. |
| InstallationPath | Used to custom set the exe/msi installer location in case it installs data outside default folder (Program Files). Path needs to be added in "Package Information" page in MSIX packaging tool during conversion. See [here](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/create-app-package#:~:text=Install%20location%3A,application%20install%20operation.) for more details.|
| Custom | Fixes that need to be done manually by the user to fix the MSIX application. Eg: Changing Application Id sequence in the app manifest. |
| PSF | Adding Package support framework fixups (Eg: FileRedirectionFixup) to fix the MSIX application. User need to create a config.json and add it and other necessary dlls in the package during conversion. See [here](https://docs.microsoft.com/en-us/windows/msix/psf/package-support-framework-overview) for more details. The author of the accelerator needs to provide the yaml equivalent of config.json in PSFConfig field. |
| Services | Services that needed to be included/excluded in order for the MSIX application to work. Need to specify in the Service Report of MSIX Packaging tool during conversion. See [here](https://docs.microsoft.com/en-us/windows/msix/packaging-tool/convert-an-installer-with-services) for more details. |
| EntryPoint | To fix issues related to EntryPoint (Eg: ShortcutNotCaptured). See [here](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-application#:~:text=Default%20value-,EntryPoint,No,-Executable) for more details. |
| InstalledLocationVirtualization | It is an extension that redirects any writes to the app's installation directory to a location in the app data. See [here](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-installedlocationvirtualization) and [here](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-updateactions) for more details. The default values for ModifiedItems, DeletedItems and AddedItems are keep, reset and keep respectively. |
| LoaderSearchPathOverride | It is an extension that allows an app developer to declare a path in the app package, relative to the app package root path, to be included in the loader search path for the app's processes. The author of the accelerator needs to provide a the list of paths to be included. See [here](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) for more details. |
