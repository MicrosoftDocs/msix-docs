---
title: Customize your Enterprise apps with modification packages
description: This article describes how tocustomize your Enterprise apps by using modification MSIX packages that store customizations.
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Customize your Enterprise apps with modification packages

The ability to customize an application's experience is important, especially for enterprises. We’ve spoken to IT professionals and we know that customizing applications to meet their user's needs is essential to the effort of moving to Windows 10. When customizing applications that are packaged using MSI, it is well understood that IT professionals must acquire the package from the developers and re-package the installer with the customization to suit their needs. This is a costly effort for enterprises. Moving forward, we want to decouple the customization and the main application so that re-packaging is no longer needed. This ensures that enterprises get the latest updates from developers while still maintaining control of their customizations.

In Windows 10, version 1809, we introduced a new type of MSIX package called a *modification package*. Modification packages are MSIX packages that store customizations. Modification packages can also be plugins/add-ons that may not have an activation point. IT professionals can use this feature to flexibly change MSIX containers so that applications are overlaid by their enterprise's customizations.

## How it works

Modification packages are designed for enterprises that do not own the code of the application and only have the installer. You can create a modification package by using the latest version of the MSIX packaging tool (for Windows 10 version 1809 or later). If you have the code for the application, you can alternatively create an [app extension](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension). 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

If you want to create a modification package that has a strict binding to the main app, you can declare the main app as a dependency in the modification package’s manifest. 

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App"/>
</Dependencies>
```

The following example demonstrates how to specify a different certificate or publisher.

``` xml
<Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
    <uap4:MainPackageDependency Name="Main.App" Publisher="CN=Contoso, C=US" />
</Dependencies>

```

This is a simple configuration if the relationship between the modification package and main package is one-to-one. Typical customizations often require registry keys under HKEY_CURRENT_USER or HKEY_CURRENT_USERCLASS. Inside our MSIX package we have User.dat and Userclass.dat files to capture the registry keys. You will need to create User.dat if you need registry keys under HKCU\Software\* (just like Registry.dat is used for HKLM\Software\*). Use Userclass.dat if you need keys under HKCU\Sofware\Classes\*. 

Here are the typical ways to create a .dat file:

* Use Regedit to create a file. Create a hive in Regedit and insert the necessary keys. Than right click, export and save-as hive file. Make sure to name the file either User.dat or Userclass.dat

* Use an API to create necessary files. You can use the [ORSaveHive](https://docs.microsoft.com/windows/win32/devnotes/orsavehive) function to save a .dat file. Make sure to name the file ether User.dat or Userclass.dat

After you’ve made the necessary changes, you can create the modification package like any other MSIX package. Then you can deploy the package with the current deployment set-up. When you relaunch your main app, you can see the changes that the modification package has made. If you choose to remove the modification package, your main app will revert to a state without the modification package. 

## Find out what modification packages are installed on your device

Using PowerShell, you can see installed modification packages using the following command.

```powershell
Get-AppPackage -PackageTypeFilter Optional
```

## Modification packages on Windows 10, version 1809

On Windows 10, version 1809, modification packages can include configurations needed to be set in the registry such that the main package will run as expected. Meaning your main application leverages the registry to view whether a plug-in exists. When you deploy the main package and the modification package, at runtime the application will view the virtual registry (VREG) of both the main package and the modification package.

Note that your main package may be using the VREG to do the following things:

* Viewing where to load the file (the DLL) of the plug-in. If this is the case, then ensure that the file is part of the package. By doing this, main package is able to access the file at runtime.
* Viewing where to see the value of the VREG keys. Your main package may be looking for a value to exist in the VREG. When you create your modification package either by hand or using our [tool](https://www.microsoft.com/p/msix-packaging-tool/9n5lw3jbcxkf), ensure that the value is correct.

## Modification packages on Windows 10, version 1903, and later

The following features were added to Windows 10, version 1903.

### Manifest update

We’ve added support for the following element to the MSIX modification package’s manifest.

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

To ensure that modification packages work in version 1903 or later, the modification package's manifest must include this element. This will be done for you if you package your MSIX modification package using the January release of the MSIX packaging tool. If you've converted a package using our tool prior to the release, you can edit your existing package in our tool to add this new element. In addition, if users install the modification package, they will be alerted that the package may modify the main application.

If you are using a modification package that was created before version 1903, it is necessary to edit the package manifest to update the `MaxVersionTested` attribute to 10.0.18362.0.

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

### Create a modification package using the MSIX Packaging Tool

You can create a modification package with the MSIX Packaging Tool:

* Specify the main package. Be sure to have the MSIX version of your main package available on your machine that you are converting on. If not than we will ask you to manually provide the publisher and main application information. Also some customization require that your main application is installed on your machine.
![Modification Package MPT](images/MPT-mod-page.png)

* Modify the package once it has gone through conversion using the package editor. There may be a case where the main package requires your modification package to have certain values in their VREG. This is where you would go and edit the package appropriately.

### Create a modification package using MakeAppx.exe

You can create a modification package manually by using the [MakeAppX.exe](package/create-app-package-with-makeappx-tool.md) tool that is included in the Windows 10 SDK.

* In the manifest, specify the main package. Include the publisher and the main package name.

    ```xml
    <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="12.0.0.0"/>
      <uap4:MainPackageDependency Name="HeadTrax" Publisher="CN=Contoso Software, O=Contoso Corporation, C=US" />
    </Dependencies>
    ```

* Create Registry.dat, User.dat and Userclass.dat to create whatever registry keys are needed to load your modification package. This is only required if you need your main application to view custom registry keys. Remember that since everything is running inside a container, at runtime the main package and the modification package virtual registry will merge such that main package can view the modification packages virtual registry.  

This process also supports file system plug-ins and customizations, as long as the executable of the main application is not in a virtual file system (VFS). This is to ensure that the main package will get all the VFS of the main package and the modification package.
