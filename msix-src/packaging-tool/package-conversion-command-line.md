---
title: Create a package using the command line interface
description: This article describes how to create an MSIX package using the command line interface for the MSIX Packaging Tool.
ms.date: 02/11/2019
ms.topic: article
keywords: windows 10, uwp
ms.custom: RS5
---

# Conversion with the command line

## Automate conversion of Windows installers to MSIX packages using scripts

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

The MSIX Packaging Tool includes the `MsixPackagingToolCLI.exe` command-line interface for
creating MSIX packages. This enables you to automate the process of repackaging app installers
and perform bulk conversions.

> [!NOTE]
> The Windows App Development CLI (`winapp`) can create an MSIX package from a prepared
> application directory, but it doesn't convert an existing installer. Use the MSIX Packaging
> Tool command-line interface for installer conversion. To package files from a prepared
> application directory, see
> [Windows App Development CLI](/windows/apps/dev-tools/winapp-cli/).

For sample PowerShell and Bash scripts that demonstrate how to automate the process of packaging,
signing, managing, and distributing MSIX packages, see the
[scripts](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts) folder of the MSIX Toolkit.

## Use the command line with the Command Prompt

To create a new MSIX package, run the `MsixPackagingToolCLI.exe create-package` command in an
elevated Command Prompt window. This command uses an
[app execution alias](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-an-alias)
for the MSIX Packaging Tool command-line interface, not the MSIX Packaging Tool GUI app.

Here are the parameters that can be passed as command-line arguments (case-sensitive):

|**Parameter** |	**Description**|
|---------|---------|
|-? --help	|Show help information|
|--template	| [required] path to the conversion template XML file containing package information and settings for this conversion|
|--virtualMachinePassword   | [optional] The password for the Virtual Machine to be used for the conversion environment. Note: The template file must contain a VirtualMachine element and the Settings::AllowPromptForPassword attribute must not be set to true.|
|--machinePassword  |[optional] The password for the remote machine to be used for the conversion environment. Note: The template file must contain a RemoteMachine or VirtualMachine element and the Settings::AllowPromptForPassword attribute must not be set to true.|
|--resume   |[optional] Used to resume the conversion flow after a reboot.|
|-v --verbose	|[optional] Print verbose logs to the console.|

Examples:

```console
MsixPackagingToolCLI.exe create-package ^
    --template C:\Users\Documents\ConversionTemplate.xml ^
    -v

MsixPackagingToolCLI.exe create-package ^
    --template C:\Users\Documents\ConversionTemplate.xml ^
    --virtualMachinePassword pswd112893
```

> [!NOTE]
> The command-line interface currently supports converting App-V 5.x packages.

You can [generate a command-line template file](generate-template-file.md) with the MSIX
Packaging Tool GUI app by completing the conversion process for an installer. You can also build
one from the sample template.
