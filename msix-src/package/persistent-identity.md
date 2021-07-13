---
title: MSIX Persistent Identity 
description: This article describes how to sign a package with a different certificate while maintaining the app update experience
ms.date: 07/02/2021
ms.topic: article
keywords: windows 10, msix, uwp, desktop, package
ms.localizationpriority: medium
---

# MSIX Persistent Identity 
This feature enables the ability to sign packages with a new certificate while still maintaining the app’s update experience. In other words, this allows the package to persist their old package identity in the platform without having the need to sign with the old (original) certificate. Starting in Windows 10 Insider Preview Build 22000, an artifact will need to be created to show the relationship between the old certificate and the new certificate that is being used for signing. Below is a step-by-step explanation of how to persist with the package identity to maintain the update experience. 

## Requirements
1. Obtain MakeAppx.exe through the Windows SDK. This feature is currently avaliable in the Windows SDK Preview. 
1. Obtain SighTool.exe through the Windows SDK 
1. The old certificate (CN=Old) that was used to sign the original package  
1. The new certificate (CN=New) that will be used to sign the package

## Walk thru
This is a step by step instructions on how to sign your package with the new certificate while maintaining the package identity. 

### Create the artifact 
1. Write the XML artifact detailing the old and new publishers. Name it anything you like, e.g. artifact.xml:
 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<Publisher xmlns=http://schemas.microsoft.com/appx/publisherbridging/2021 Old="CN=Old" New="CN=New" />
```
2. Write a Catalog Definition File (CDF) to create the catalog that will be used to sign the artifact. Name it anything you like, e.g. artifact.cdf:
 ```
[CatalogHeader]
Name=artifact.cat
CatalogVersion=2
HashAlgorithms=SHA256
 
[CatalogFiles]
<HASH>artifact.xml=artifact.xml
```
3. Run makecat.exe on this CDF. It will create the file specified in it
```powershell
makecat.exe artifact.cdf
```
4. Sign the catalog using the old certificate
```powershell
signtool.exe sign /f old-cert.pfx /fd SHA256 artifact.cat
```
At this point, you only need to keep the XML and CAT files after this. You can create several artifacts but we currently support up to 5 at the moment.

### Create the package
1.	Create a publisher bridging file to tell makeappx what artifacts to use. This file is similar to the mapping file. Name it anything you like, e.g. artifacts.txt
 ```
[PublisherBridging]
"artifact.xml" "artifact.cat"
```
Each line has to contain a pair of XML and CAT file paths. The artifacts must be ordered as they are applied. E.g., if you have two artifacts one for going **Publisher1->Publisher2**, and another for **Publisher2->Publisher3**, you must list first the one for **Publisher1->Publisher2**
2. Call makeappx.exe with  **/pb** flag to point to the publisher bridging file:
```powershell
makeappx.exe /p app.msix /d .\app\ /pb artifacts.txt
 ```
3. Sign your package using the new certificate
```powershell 
signtool.exe sign /f new-cert.pfx /fd SHA256 app.msix
```

Now you have a package that has the artificate stored inside of it and been signed with the new certificate. You can deploy the package like any other MSIX package. 

### Considerations
1. We recommend that catalog should be timestamped. To do this you would need to add these arguments in the call to signtool before the path to the catalog: /td SHA256 /tr <URL to time stamp server>

1. You will still need to install the old certificate (recommended with timestamp) on the machine in order for the platform to install the package that was signed by the new certificate. 
 
1. This feature works for both MSIX packages and MSIX Bundles 
