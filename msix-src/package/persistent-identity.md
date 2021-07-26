---
title: MSIX Persistent Identity 
description: This article describes how to sign a package with a different certificate while maintaining the app update experience
ms.date: 07/02/2021
ms.topic: article
keywords: windows 10, msix, uwp, desktop, package
ms.localizationpriority: medium
---

# MSIX persistent identity 
This feature enables the ability to sign packages with a new certificate while still maintaining the app’s update experience. In other words, this allows the package to persist their old package identity in the platform without having the need to sign with the old (original) certificate. Starting in Windows Insider Preview Build 22000, an artifact will need to be created to show the relationship between the old certificate and the new certificate that is being used for signing. Below is a step-by-step explanation of how to persist with the package identity to maintain the update experience. 

## Requirements
- Obtain MakeAppx.exe through the Windows SDK. This feature is currently available in the Windows SDK Preview 22000
- Obtain SignTool.exe through the Windows SDK. This feature is currently available in the Windows SDK Preview 22000
- The old certificate (CN=Old) that was used to sign the original package  
- The new certificate (CN=New) that will be used to sign the package

## Walk through
This is a step-by-step instruction on how to sign your package with the new certificate while maintaining the package identity.

### Create the artifact 
1. Write the XML artifact detailing the old and new publishers. Name it anything you like, artifact.xml:
 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<Publisher xmlns=http://schemas.microsoft.com/appx/publisherbridging/2021 Old="CN=Old" New="CN=New" />
```
2. Write a Catalog Definition File (CDF) to create the catalog that will be used to sign the artifact. Name it anything you like, artifact.cdf:
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
At this point, you only need to keep the XML and CAT files after this. You can create several artifacts, however the platform currently supports up to 5.

### Create the package
1.	Create a publisher bridging file to tell makeappx.exe what artifacts to use. This file is like the mapping file. Name it anything you like, artifacts.txt
 ```
[PublisherBridging]
"artifact.xml" "artifact.cat"
```
Each line must contain a pair of XML and CAT file paths. The artifacts must be ordered as they are applied. If you have two artifacts one for going **Publisher1->Publisher2**, and another for **Publisher2->Publisher3**, you must list first the one for **Publisher1->Publisher2**

2. Call makeappx.exe with  **/pb** flag to point to the publisher bridging file:
```powershell
makeappx.exe /p app.msix /d .\app\ /pb artifacts.txt
 ```
3. Sign your package using the new certificate
```powershell 
signtool.exe sign /f new-cert.pfx /fd SHA256 app.msix
```

Now you have a package that has the artifact(s) stored inside of it and been signed with the new certificate. You can deploy the package like any other MSIX package. 

### Considerations
- We recommend that the catalog should be timestamped. To do this you would need to add these arguments in the call to signtool before the path to the catalog: /td SHA256 /tr

- You will still need to install the old certificate (recommended with timestamp) on the machine for the platform to install the package that was signed by the new certificate.

- In order to leverage this feature, you will need to do this before the old certificate has expired. 
 
- This feature works for both MSIX packages and MSIX Bundles 
