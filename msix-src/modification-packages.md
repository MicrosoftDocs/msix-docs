---
title: Customize your Enterprise apps with modification packages
description: Learn how to customize your Enterprise apps
ms.date: 01/15/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Customize your Enterprise apps with modification packages 

The ability to customize an application's experience is important, especially for enterprises. We’ve spoken to IT professionals and we know that customizing applications to meet their user's needs is essential to the effort of moving to Windows 10. When customizing applications that are packaged using MSI, it is well understood that IT professionals must acquire the package from the developers and re-package the installer with the customization to suit their needs. This is a costly effort for enterprises. Moving forward, we want to decouple the customization and the main application so that re-packaging is no longer needed. This ensures that enterprises get the latest updates from developers while still maintaining control of their customizations.

In Windows 10 version 1809 we introduced a new type of MSIX package called a *modification package*. Modification packages are MSIX packages that store customizations. Modification packages can also be plugins/add-ons that may not have an activation point. IT professionals can use this feature to flexibly change MSIX containers so that applications are overlaid by their enterprise's customizations. We are excited about this feature and look forward to introducing enhancements in future releases. 

## How it works

Modification packages are designed for enterprises that do not own the code of the application and only have the installer. You can create a modification package by using the latest version of the MSIX packaging tool (for Windows 10 version 1809 or later). If you have the code for the application, you can alternatively create an [app extension](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-an-extension). 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Get MSIX Packaging Tool</a></p></div>

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

1.  Use Regedit to create a file. Create a hive in Regedit and insert the necessary keys. Than right click, export and save-as hive file. Make sure to name the file either User.dat or Userclass.dat

2.  Use an API to create necessary files. We’ve published an [API](https://msdn.microsoft.com/en-us/library/ee210773(v=vs.85).aspx) that you can use to save a .dat file. Make sure to name the file ether User.dat or Userclass.dat

After you’ve made the necessary changes, you can create the modification package like any other MSIX package. Then you can deploy the package with the current deployment set-up. When you relaunch your main app, you can see the changes that the modification package has made. If you choose to remove the modification package, your main app will revert to a state without the modification package. 

## See also
- [Modification packages on Windows 10 version 1809](modification-package-1809-update.md)
- [Modification packages on Windows 10 version 1903](modification-package-1903.md)
