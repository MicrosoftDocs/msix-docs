---
description: This article provides guidance on how to run, debug, and test your packaged desktop application to get it ready for deployment.
title: Run, debug, and test an MSIX package
ms.date: 02/03/2020
ms.topic: article
keywords: windows 10, uwp, msix
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
---

# Run, debug, and test an MSIX package

Run your packaged application and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your application in a production environment, sign your application and then install it. This topic shows you how to do each of these things.

<a id="run-app"></a>

## Run your application

You can run your application to test it out locally without having to obtain a certificate and sign it. How you run the application depends on what tool you used to create the package.

### You created the package by using Visual Studio

Set the packaging project as the startup project, and then press F5 to start your app.

### You created the package using a different tool

Open a Windows PowerShell command prompt, and from the root directory of your package files, run this cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
To start your app, find it in the Windows Start menu.

![Packaged application in the start menu](images/converted-app-installed.png)

> [!NOTE]
> A packaged application always runs as an interactive user, and any drive that you install your packaged application on to must be formatted to NTFS format.

## Debug your app

How you debug the application depends on what tool you used to create the package.

If you created your package by using the [new packaging project](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) available in Visual Studio 2017 version 15.4 and later (including Visual Studio 2019), just set the packaging project as the startup project, and then press F5 to debug your app.

If you created your package using any other tool, follow these steps:

1. Make sure that you start your packaged application at least one time so that it's installed on your local machine.

   See the [Run your app](#run-app) section above.

2. Start Visual Studio.

   If you want to debug your application with elevated permissions, start Visual Studio by using the **Run as Administrator** option.

3. In Visual Studio, choose **Debug**->**Other Debug Targets**->**Debug Installed App Package**.

4. In the **Installed App Packages** list, select your app package, and then choose the **Attach** button.

#### Modify your application in between debug sessions

If you make your changes to your application to fix bugs, repackage it by using the MakeAppx tool. See [Run the MakeAppx tool](desktop-to-uwp-manual-conversion.md#make-appx).

### Debug the entire application lifecycle

In some cases, you might want finer-grained control over the debugging process, including the ability to debug your application before it starts.

You can use [PLMDebug](/windows-hardware/drivers/debugger/plmdebug) to get full control over application lifecycle including suspending, resuming, and termination.

[PLMDebug](/windows-hardware/drivers/debugger/plmdebug) is included with the Windows SDK.

## Test your app

To deploy your packaged application for end-to-end production testing as you prepare for distribution, you need to sign your package with a certificate that is trusted on the machine you're deploying the app.

### Test an application that you packaged by using Visual Studio

Visual Studio signs your application by using a test certificate. You'll find that certificate in the output folder that the **Create App Packages** wizard generates. The certificate file has the *.cer* extension and you'll have to install that certificate into the **Trusted People** certificate store on the PC that you want to test your application on. See [Package a desktop or UWP app in Visual Studio](../package/packaging-uwp-apps.md#generate-an-app-package).

### Test an application that you packaged using a different tool

If you package your application outside of Visual Studio you can sign your application package using the Sign Tool. If the cert you used for signing is not trusted on the machine you're testing on, you'll need to install the cert to Trusted People certificate store before installing the app package. 

#### Sign your application package

To manually sign your application package:

1. Create a certificate. See [Create a certificate](../package/create-certificate-package-signing.md).

2. Install that certificate into the **Trusted People** certificate store on your system.

3. Sign your application by using that certificate, see [Sign an app package using SignTool](../package/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Make sure that the publisher name on your certificate matches the publisher name of your app.

**Related sample**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)

### Test your application with comparepackage.exe
ComparePackage.exe is a tool in the [Windows SDK](https://developer.microsoft.com/windows/downloads/windows-sdk/) that shows a report that states which files have been changed, what new files have been added, what files have been deleted, and what remains unchanged when an app has been update from one version to the next.

### Test your app using Local App Attach

Local App Attach allows you to run MSIX applications without installing them on the device. The APIs that power Local App Attach are fully supported on Windows 11 Enterprise and Windows 10 Enterprise, baked into the OS to mount and unmount the applications. You can also use PowerShell cmdlets or scripts to automate the process. For more information, see [Test MSIX packages for app attach](/azure/virtual-desktop/app-attach-test-msix-packages). 

### Test your application for Windows 10 S

Before you publish your app, make sure that it will operate correctly on devices that run Windows 10 S. In fact, if you plan to publish your application to the Microsoft Store, you must do this because it is a store requirement. Apps that don't operate correctly on devices that run Windows 10 S won't be certified.

See [Test your Windows application for Windows 10 S](desktop-to-uwp-test-windows-s.md).

### Run another process inside the full trust container

You can invoke custom processes inside the container of a specified app package. This can be useful for testing scenarios (for example, if you have a custom test harness and want to test output of the app). To do so, use the ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## Next steps

Have questions? Ask us on the [MSIX Tech Community](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX).
