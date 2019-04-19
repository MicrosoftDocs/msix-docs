---
title: Create an application package with all other installers
description: Create a MSIX package with our conversion tool with all other installers
author: dianmsft
ms.author: diahar
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool
ms.localizationpriority: medium
---

# Create an MSIX package with all other installer types

You can use the MSIX Packaging Tool to create an MSIX application package for any installer types. The device or VM that you are using to convert your apps to MSIX must meet the following criteria:

- It must be configured to receive remote commands (run the Enable-PSRemoting command on the VM).
- It must be running Windows 10, version 1809, or a later version of Windows.
- When the tool is first launched, you will be prompted to provide consent to sending telemetry data. It's important to note that the diagnostic data you share only comes from the app and is never used to identify or contact you. This just helps us fix things faster for you.

## Choose the installer you want to package

Under **Choose the installer you want to package**, find the installer on your machine. If you do not have an installer, you may skip this step. We do recommend that you sign your package.

- Check the box under **Sign package**, browse to and select your .pfx certificate file. If the certificate is password protected, type the password in the password box.
- Check the box under **Use installer arguments** and enter the desired argument in the provided field. This is optional. This field accepts any string.

## Packaging method

- Select your packaging method and click **Next**.

## Package information

After you choose to package your application on an existing virtual machine, you must provide information about to the app. The tool will try to auto-fill these fields based on the information available from the installer (if applicable). You will always have a choice to update the entries as needed. If the field as an asterisk*, it's required. Inline help is provided if the entry is not valid.

- Package name:
  - Required and corresponds to package identity Name in the manifest to describe the contents of the package.
  - Must match the Name subject information of the certificate used to sign a package.
  - Is not shown to the end user.
  - Is case-sensitive and cannot have a space.
  - Can accept string between 3 and 50 characters in length that consists of alpha-numeric, period, and dash characters.
  - Cannot end with a period and be one of these: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8", and "LPT9."

- Package display name:
  - Required and corresponds to package in the manifest to display a friendly package name to the user, in start menu and settings pages.
  - Field accepts A string between 1 and 256 characters in length and is localizable.

- Publisher name:
  - Required and corresponds to package that describes the publisher information.
  - The Publisher attribute must match the publisher subject information of the certificate used to sign a package.
  - This field accepts a string between 1 and 8192 characters in length that fits the regular expression of a distinguished name : "(CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")(, ((CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")))*".

- Publisher display name:

  - Required and corresponds to package in the manifest to display a friendly publisher name to the user, in App installer and settings pages.
  - Field accepts A string between 1 and 256 characters in length and is localizable.

- Version:

  - Required and corresponds to the package in the manifest to describe the version number of the package.
  - This field accepts a version string in quad notation: "Major.Minor.Build.Revision".

- Install location:

  - This is the location that the installer is going to copy the application payload to (usually Programs Files folder).
  - This field is optional but recommended specially when app payload is being installed outside of the Program Files folders.
  - Browse to and select a folder path.
  - Make sure this file matches the installer's install location while you go through the application install operation.

## Prepare computer

Next, the **Prepare computer** page provides options to prepare the computer for packaging.

The MSIX Packaging Tool Driver is required and the tool will automatically try to enable it if it is not enabled. The tool will first check with DISM to see if the driver is installed.

> [!NOTE]
> The MSIX Packaging Tool Driver monitors the system to capture the changes that an installer is making on the system which allows MSIX Packaging Tool to create a package based on those changes.

- [Optional] Check the box for **Windows Search is Active** and select **Disable selected** if you choose to disable the search service.

  - This is not required, only recommended.
  - Once disabled, the tool will update the status field to “disabled”

- [Optional] Check the box for **Windows Update is Active** and select **Disable selected** if you choose to disable the Update service.

  - This is not required, only recommended.
  - Once disabled, the tool will update the status field to **Disabled**.

- The **Pending reboot** checkbox is disabled by default. You'll need to manually restart the machine and then launch the tool again if you are prompted that pending operations need a reboot. This not required, only recommended.

When you're done preparing the machine, click **Next**.

## Installation

At this time, if you did not provide us with an installer, go ahead and run any and all required installers at this time. This includes custom scripts and programs. The tool is monitoring changes on the device, including things that are being installed on the device.

## Manage first launch tasks

This page shows application executables that the tool captured. If there are multiple applications, check the box that corresponds to the main entry point. If you don't see the application .exe here, manually browse to and run it.

Click **Next** You'll be prompted with a pop up asking for confirmation that you're finished with application installation and managing first launch tasks.

- If you're done, click **Yes, move on**.
- If you're not done, click **No, I'm not done**. You'll be taken back to the last page to where you can launch applications, install or copy other files, and dlls/executables.

## Create package

- Provide a location to save the MSIX package.
- The default save location is the desktop.
- You can define the default save location in Settings menu.
- If you'd like to continue to edit the content and properties of the package before saving the MSIX package, you can select **Package editor** and be taken to [package editor]("https://docs.microsoft.com/en-us/windows/msix/packaging-tool/package-editor")
- Click **Create** to create the MSIX package.

You'll be presented with the pop up when the package is created. This pop up will include the name, publisher, save location of logs and save location of the newly created package. You can close this pop up and get redirected to the welcome page. You can also select package editor to see and modify the package content and properties.