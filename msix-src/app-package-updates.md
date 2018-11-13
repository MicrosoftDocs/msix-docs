---
title: App package updates | Microsoft Docs
description: Learn how apps are differentially updated.
author: laurenhughes
ms.author: lahugh
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
---

# App package updates

Updating modern Windows app packages is optimized to ensure that only the essential changed bits of the app are downloaded to update an existing Windows app..

At a high level, during package creation, a piece of metadata is created and stored in the app package file (.appx or .msix) which allows parts of the package to be uniquely identified by Windows. When updating an app package, Windows uses the metadata file to compare the old package to the new package and determine what needs to be downloaded to the device.

Since the metadata allows parts of the package to be uniquely identified, this means the differential update machinery fully functions from any version of a package to any other version of a package (assuming the source package has a lower version than the target package). 

The metadata is contained in the AppxBlockMap.xml file (the aforementioned metadata). The AppxBlockMap.xml file is an XML file that contains a two dimensional list of information about files in the package. The first dimension lays out high level details on the file (e.g., name and size) and the second dimension provides SHA2-256 hash representations of each 64KB slice of that file (the "block").

Here's a sample of an AppxBlockMap.xml file.

```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<blockmap hashmethod="http://www.w3.org/2001/04/xmlenc#sha256" 
          xmlns="http://schemas.microsoft.com/appx/2010/blockmap">
  <file lfhsize="66" size="101188" name="asset1.jpg">
    <block hash="2bidNE0JyaO+FjaTpRe0g8HzUCblUf/cfBcTXiZR74c="/>
    <block hash="+jeFwKrGk5gw9wSICWsWRtEQXwcLC7af4EWS7DgrAkY="/>
  </file>
  <file lfhsize="61" size="108823" name="asset2.jpg">
    <block hash="u0+5S0GOzwyAfYx54tKycZyHRBYm2ybvq27dkIKqDsQ="/>
    <block hash="F9h0FRMetL6BNCszAYB0bgyx2KWN+dO1bls4Q9m267c="/>
  </file>
  ...
</blockmap>
```

The first file (asset1.jpg) has two block hashes. The first hash represents the first 64KB block of the file and the second hash represents the remaining 35KB - given the file is 101188 bytes.

During an update, if the second block of that file is modified, the hash is also updated to reflect that. The download component pulls down the second block and reuses the first unchanged block from the old package.

On a larger scale, if an entire file does not change (determined by a full set of blocks not changing), that file can be reused from the existing package, saving time and resources.

There are a couple of ways to employ this differential update technology.

- Keep files in the package small - doing this will ensure that if a change is needed that would impact the full file, the update would still be small.
- Modifications to files should be additive if possible - additive changes will ensure that end-user devices only download those changed blocks.
- Modifications to files should be contained to 64KB blocks if possible - if your app does have large files and requires changes to the middle of a file, containing changes to a set of blocks will help significantly.
