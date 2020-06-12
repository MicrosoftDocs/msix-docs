---
title: MSIX Packaging Tool Known Issues and Troubleshooting Tips
description: Describes known issues and provides troubleshooting tips to consider when converting your apps to MSIX using the MSIX Packaging Tool. 
ms.date: 06/11/2020
ms.topic: article
keywords: msix packaging tool, known issues, troubleshooting
ms.localizationpriority: medium
ms.custom: RS5
---

# Known issues and troubleshooting tips for the MSIX Packaging Tool

This article describes known issues and provides troubleshooting tips to consider when converting your apps to MSIX using the MSIX Packaging Tool. Check out our other docs if you need to acquire the MSIX Packaging Tool or driver in a [disconnected environments](disconnected-environment.md).

## Known issues

### Getting the latest Insider Preview build of the MSIX Packaging Tool

If you have opted in to our [Insider Program](insider-program.md), make sure you have the correct version of the MSIX Packaging Tool:
- Go to the **About** section in the MSIX Packaging Tool to view which version you are on.
- Go [here](insider-program.md#current-insider-preview-build) to determine the latest Insider Preview version, and confirm you have that version of the MSIX Packaging Tool installed. 
- Make sure the MSA that's signed up for flighting is the account that is signed into the Microsoft Store. 
- Manually update the MSIX Packaging Tool through the Microsoft Store on your computer. If this option if available to you, open the Store, go to **Downloads and updates**, and click **Get updates**. Alternatively, search for the MSIX Packaging Tool, and on the product page you can then prompt it for an update. 
- To install the MSIX Packaging Tool for offline use, follow [these instructions](disconnected-environment.md#get-the-msix-packaging-tool) to ensure you get the latest app through our offline process.

If you are interested in joining our Insider Program, click [here](https://aka.ms/MSIXPackagingPreviewProgram).

### MSIX Packaging Tool driver

The MSIX Packaging Tool driver is delivered as a [Feature on Demand (FOD)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) package from Windows Update. It will fail to install if the Windows Update service is disabled on the computer or if Windows Insider flight ring settings do not match the OS build of the computer.

If you are running into problems acquiring the driver, or you are working in an offline environment, you can find links to download the driver [here](disconnected-environment.md#get-the-msix-packaging-tool-driver). 

If you have downloaded the driver and are running into issues during your package conversion, it may be one of the following issues.

#### Network connectivity issues

The following error codes might indicate that you are encountering connection issues:

* -2145107924 (0x8024402c)
* -2145107945 (0x80244017)
* -2145123272 (0x80240438)

#### Windows Server Update Service (WSUS), Configuration Manager, or group policies affecting Windows Update connectivity

The following error code may indicate that policies on your computer are be affecting Windows Update connectivity: -2145124306 (0x8024002e).

If you receive this error code, you may need to check your [environment settings and policies](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

#### Driver required a reinstall

In this scenario, the MSIX Packaging Tool will notify you in the error message and logs that your driver needs a restart. Restart your computer and start your conversion again to fix this issue. 

### Minimum version

There are a few features to be aware of that automatically change the minimun version support in your MSIX Package. 

#### Enforce Microsoft store versioning requirements
If you convert your existing installer using a version of the [MSIX Packaging Tool](tool-overview.md) earlier than **1.2019.701.0**, the tool had Enforce Microsoft Store versioning requirements on, or used another tool to create your package that did not set the minimum version to 10.0.16299.0 (Windows 10, version 1709). This will cause an error message when deploying your app to Windows 10, version 1709 or a later version.

To fix this issue, open the **MSIX Packaging Tool** and edit your app through **Package Editor**. Open your manifest and set the `MinVersion` attribute of the `TargetDeviceFamily` element to "10.0.16299.0".

```xml
<Dependencies>
    <TargetDeviceFamily> Name="Windows.Desktop" MinVersion="10.0.16299.0" MaxVersionTested = "10.0.17763.0" />
</Dependencies>
```

#### MSIX with services
In version 1.2019.1220.0 of the MSIX Packaging Tool, we added support for creating an [MSIX package with Services](convert-an-installer-with-services.md). Due to the OS restrictions with services support, the MSIX Packaging Tool automatically changes the min version supported in an MSIX package with services to 10.0.19025.0. This means that you cannot install an MSIX with services on an OS lower than the Windows 10 version 2004, but you can create that MSIX using the MSIX Packaging Tool down to Windows 10 1809. If you need to install this app on a lower OS, update the min version appropriately but note that support for services will not work.

### Frameworks and drivers

If the app requires a framework, make sure the framework is installed during the monitoring phase of the conversion. Go through the logs to ensure this is happening. If your app requires a driver to install, you need to evaluate whether this is required for your app to run properly. MSIX currently does not support driver installation.

### Remote Machine
If you are running into issues with using a remote VM for your conversions, see [Setup instructions for remote machine conversions](remote-conversion-setup.md).

### Issues during conversion
- Some installers might fail to convert with exit code 259. This indicates that the installer spawned a thread and did not wait for it to complete. In other words, the main thread finished installing but it exited with error 259 because it spawned a thread that is still running. We recommend that you use the appropriate install option for setup.exe.

### Issues during signing

#### Bad PE certificate (0x800700C1)

This problem occurs when the package contains a binary file that has a corrupt certificate. To resolve this issue, use the `dumpbin.exe /headers` command to dump the file headers and inspect for bad elements. Manually rewrite the headers to fix the issue. In general, the MSIX Packaging tool automatically detects bad headers. If this issue persists,  file feedback. More information can be found [here](../desktop/desktop-to-uwp-known-issues.md#bad-pe-certificate-0x800700c1).

#### Device Guard signing

Make sure to follow [these steps](../package/signing-package-device-guard-signing.md) and that you are assigning the appropriate roles in the Microsoft Store for Business.

#### Expired certificate

- Use a timestamp when you sign your package.
- You can resign with a valid sign or timestamp certificate.

You can resign your app using the [batch conversion script](https://github.com/microsoft/MSIX-Toolkit/tree/master/Scripts).

## Troubleshooting

### Log files

Whether or not your conversion was successful, log files are generated for every conversion. They can be found here:

`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\`

Failure codes are written and indicate any point of failure during the conversion process. The error codes are meant to be user friendly.

#### Log files from remote devices or VMs

If the conversion is performed on a remote device or a VM, we recommend that you copy the log files from that device and attach them as part of the feedback item. This will help us diagnose and resolve issues more efficiently.

You will find the logs from the remote conversions here:
`%localappdata%\packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\DiagOutputDir\<Logs_#>\RemoteServer\Log.txt`

It would even more beneficial if you can share the whole Logs folder that will include the operations occurring on the local client as well the remote server.

### Common problems

#### MakePri/Manifest translation errors

This error occurs when there is an issue with the package’s manifest. To identify the issue, go to Package Editor and open the manifest. When you open the manifest, you can identify the issue and provide the proper fix.

#### File not found

The file may either be open or non-existent. To resolve this issue, add the appropriate file or close the file that is currently in use. Note that you will not get a `File not Found` error if it is open. Instead, you’ll get an `Access Denied` or `File in Use` error.

#### File Type Associations

The issues regarding File Type Associations (FTA) vary from package to package. MSIX Packaging Tool support file associations for double click installs. For example, if your app has context menu, it is not automatically added, so you will need to add it manually to the manifest. See the [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) manifest element for an example.

#### Shortcuts with arguments

Shortcuts with arguments are not currently supported with MSIX. If we detect that the installer includes these, MSIX will create a tile with no arguments.

#### Install directory

This is more common for those who use a secondary drive to perform app conversions. If you choose to change the installation location, it changes the root of where all of the files go. This means that the MSIX Packaging tool will need to know where all these files go and will be captured during conversion. 

You can fix this by using the Package Support Framework write to install directory fix. We have added this as a capability by default in the MSIX Tool, which allows this down to 1809. If your application isn't working in 1709 and is in 1809, this is likely the issue.

## Sending feedback

The best way to send your feedback is through the [**Feedback Hub**](https://www.microsoft.com/p/feedback-hub/9nblggh4r32n?activetab=pivot:overviewtab).

1. Open **Feedback Hub** or type **Windows + F**.
2. Provide a title and necessary steps to reproduce the issue.
3. Under **Category**, select **Apps** and select **MSIX Packaging Tool**.
4. Attach any [log files](#log-files) associated to the conversion. You can find the logs in the folder provided above.
5. Attach the converted MSIX package (if possible).
6. Click **Submit**.

You can also send us feedback directly from the MSIX Packaging Tool by going to the **Feedback** tab under **Settings**. 

> [!NOTE]
> It may take 24 hours for your feedback to get to us. Therefore if you are using a VM to convert your package, you may want to keep your VM on and in its current state for 24 hours after conversion. Also, you can manually attach conversion logs to the feedback.
