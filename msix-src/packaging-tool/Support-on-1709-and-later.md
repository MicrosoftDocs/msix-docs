---
title: MSIX Package support on Windows 10 version 1709 and later
description: Install MSIX packages on 1709 and later.
author: c-don
ms.author: cdon
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, 1709, 16299
ms.localizationpriority: medium
ms.custom: RS5
---


# MSIX Package support on 1709 and later

If you've converted an existing app to MSIX, you may want to use the app in earlier versions of Windows than 1809 (build 17701). This blog post discusses how to enable such apps in builds as early as 16299, also known as Windows 10 version 1709. 
 
 
## Problem:
So you converted your existing app to MSIX using the MSIX Packaging Tool and it runs fine on 1809 and later. But now that you know we are adding support for MSIX starting build 16299, you want to run your MSIX app on this or any later build. Currently, if you just try to install your MSIX app on a machine with build 16299 to 17700, you'll get this error message: 

![PowerShell MSIX install](images/mpt_blog_0.jpg)

## Solution:
We are working to update the MSIX Packaging Tool to handle this, but in the meantime here's what you can to do to set up your app to run on these builds:
 
Open the MSIX packaging tool and navigate to the package editor.

![open](images/mpt_blog_1.jpg) 
![navigate](images/mpt_blog_2.jpg)


Navigate to your MSIX package. In my case, it is the Test_App.msix package. Click "open package"

![open package](images/mpt_blog_3.jpg)

On the bottom of the "Package Information" tab, observe the option to open the Manifest file. 

![open manifest](images/mpt_blog_4.jpg)

Select "Open file" and edit MinVersion to equal 16299 as shown below:

![observe manifest](images/mpt_blog_5.jpg)
![edit manifest](images/mpt_blog_6.jpg)
![edit manifest2](images/mpt_blog_7.jpg)

Once you are done editing the manifest, close the file. This will return you to the Package Editor.
Don't forget to re-sign the edited package as shown below:

![sign](images/mpt_blog_9.jpg)

Once the update has been made, save your changes.

![save](images/mpt_blog_10.jpg)

At this point, you can install the app on a device with 16299 build or later.
 


 
## Troubleshooting tips:
For now, in devices running builds 16299 to 17700, you can install MSIX apps through PowerShell. 
Specifically, you need this command in PowerShell:
add-appxpackage <path to MSIX package>

![PowerPoint command](images/mpt_blog_11.jpg)

You can also use Intune, SCCM or the Packaging Manager API.



