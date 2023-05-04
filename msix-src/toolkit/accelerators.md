---
# Required metadata
# For more information, see https://review.learn.microsoft.com/en-us/help/platform/learn-editor-add-metadata?branch=main
# For valid values of ms.service, ms.prod, and ms.topic, see https://review.learn.microsoft.com/en-us/help/platform/metadata-taxonomies?branch=main

title:       # Add a title for the browser tab
description: # Add a meaningful description for search results
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
1. Check out the sample accelerator here at MSIX-Labs DeveloperLabs to see the accelerator structure, and use it to build your own accelerator.

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
