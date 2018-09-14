---
title: Customize your Enterprise apps with Modification Package | Microsoft Docs
description: Learn how to customize your Enterprise apps
author: laurenhughes
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# Customize your Enterprise apps with Modification Package 

MSIX is the future installer of Windows. MSIX is the best technology for installing application on Windows. It inherits UWP features, allows for application customization, more container security options and supports all Windows application. MSIX will be supported in the Microsoft store and supports enterprise management as well. 

The ability to customize application experience is important, especially in the enterprise space. We’ve spoken to IT Pros and we know that customizing applications to meet your users needs is essential to the effort of moving to Windows 10. With MSI package application, it was well understood that for IT Pros to customize applications they would have to take the package from the developers and re-package the installer with the customization to suit their needs. This is a costly effort for the enterprises. Moving, we want to decouple the customization and the main application, so re-packaging is no longer needed. This ensures that the enterprises get the latest updates from the developers while still maintaining control of their customization. So, we want to enable IT Pros to flexibly change the MSIX container, such that these applications are overlaid by the enterprises customization.  We are excited about this feature and look forward to introducing enhancements in feature releases. 

In the Fall 2018 release, we are introducing modifications packages. Modification packages are MSIX packages that store your customization. Modification packages can also be plugins/add-ons that may not have an activation point. 

## How it works
Again, modification packages are geared towards enterprises that do not own the code of the application and only have the installer. If you have the code for the application, we suggest you use app extensions model to do similar things. You can create a modification package by using the MSIX packaging tool that will be out early in Fall 2018 (or earlier with an Insider build). However, if you would like to create a modification package that has a strict binding to the main app you can declare the main app as a dependency in the modification package’s manifest. 

## Example 
``` xml
<Dependencies>
  <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.15063.0"/>
  <uap3:MainPackageDependency Name="Main.App"/>
</Dependencies>
```
This is a simple configuration if the relationship between the modification package and main package is one to one. Typical customization requires registry keys to be under HKEY_CURRENT_USER or HKEY_CURRENT_USERCLASS. Inside our MSIX package we have User.dat and Userclass.dat files to capture the registry keys. You will need to create User.dat if you need registry keys under HKCU\Software\* (just like Registry.dat is used for HKLM\Software\*). Use Userclass.dat if you need keys under HKCU\Sofware\Classes\*. 

Here are typically ways of which you can create a .dat file: 
1.	Use Regedit to create a file: Simply create a hive in Regedit and insert the necessary keys. Than right click, export and save-as hive file. Make sure to name the file either User.dat or Userclass.dat
2.	Use API to create necessary files: We’ve published an API that you can use to save a .dat file. https://msdn.microsoft.com/en-us/library/ee210773(v=vs.85).aspx Make sure to name the file ether User.dat or Userclass.dat

Once you’ve made the necessary changes, you can create the modification package like any other MSIX package. Then, you can deploy the package with the current deployment set-up. When you relaunch your main app, you can see the changes that the modification package has made. If you choose to remove the modification package, your main app will revert to a state without the modification package. 


<br>
<br>
<div class="container centered pageFooter">
        <h2>Have feedback for us? We'd love to hear it.</h2>
        <ul class="links">
           <li>
                <a href="mailto:MSIXWebsiteFeedback@service.microsoft.com" data-linktype="external">
                    Email the MSIX team
                </a>
            </li>
           
        </ul>
		</div>

<!--
 <div class="container centered pageFooter">
        <h2>Keep in touch with us</h2>
        <ul class="links">
           <li>
                <a href="https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX">
                    MSIX tech community
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/MSIX-PackageSupportFramework/issues">
                    Package Support Framework
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/msix-packaging/issues">
                    MSIX SDK
                </a>
            </li>
            <li>
                <a href="https://twitter.com/#!/search/realtime/%23msix">
                    Twitter
                </a>
            </li>
            
        </ul>
		</div>
-->

