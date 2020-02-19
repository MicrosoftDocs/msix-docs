---
title: Create a package using the command line interface
description: This article describes how to create an MSIX package using the command line interface for the MSIX Packaging Tool.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Conversion with the command line

## Automate conversion of Windows installers to MSIX packages using scripts

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool supports a command line interface for creating MSIX application packages. This enables you to automate the process of repackaging app installers and perform bulk conversions.

For sample PowerShell and Bash scripts that demonstrate how to automate the process of packaging, signing, managing and distributing MSIX packages, see the [scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) folder of the MSIX Toolkit.

## Use the command line with the Command Prompt

To create a new MSIX package for your application, run the `MsixPackagingTool.exe create-package` command in an administrator Command prompt window.

Here are the parameters that can be passed as command line arguments:

|**Parameter** |	**Description**|
|---------|---------|
|-? --help	|Show help information|
|--template	| [required] path to the conversion template XML file containing package information and settings for this conversion|
|--virtualMachinePassword	| [optional] The password for the Virtual Machine to be used for the conversion environment. Notes: The template file must contain a VirtualMachine element and the Settings::AllowPromptForPassword attribute must not be set to true.|
|-v --verbose	|[optional] Print verbose logs to the console.|

Examples:

```console

    MsixPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml -v

    MSIXPackagingTool.exe create-package --template c:\users\documents\ConversionTemplate.xml --virtualMachinePassword pswd112893
    
```

> [!NOTE]
> App-V 5.x conversion is currently supported to be converted throught the command line. This includes capabilities.

You can [generate a command line template file](generate-template-file.md) through the MSIX Packaging Tool by going through the conversion process with an application or you can build one from our sample template.
