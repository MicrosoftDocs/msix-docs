---
description: Enforce a packag integrity check at run time 
title: Enforce Package Integrity Check
author: dianmsft
ms.date: 03/25/2021
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
---

# Enforce Package Integrity Check
Windows is able to run time package integrity checks on the entire contents of the package. If enabled, Windows will perform run time checks and initiate a package remediation and repair workflow before launching the app if it detects a tampered or corrupt package.

## How to enable this 
In the package manifest , insert the following element: 

```xml
<uap10:PackageIntegrity>

  <!-- Child elements -->
  <uap10:Content Enforcement="on" />

</uap10:PackageIntegrity>
```

By checking indicating that **Enforcement** is **on**, this will indicate that Windows will enforce run time package integrity checks on the entire contents of the package. There are three values that **Enforcement** can be, **on**, **off** or **default**. The value **default** is the same behavior as **off** 

## User experience
When package integrity is checked and the system identifies that the package files have been tampered, depending on the source of the package, a dialog will appear to the user indicating that there is an issue with the app. If the app came from the store, the user will be directed to take action via the Store app. If the app came from outside the Microsoft Store, the user dialog will be generic. The user will be prompted to go to the Settings app and either **Repair** or **Reset** the app. 
