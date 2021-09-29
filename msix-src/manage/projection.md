---
description: This guide the ability to project files 
title: Ability to project files in MSIX packages
ms.date: 03/17/2021
ms.topic: article
keywords: msix
ms.localizationpriority: medium
---

# Ability to project files in MSIX packages
In order to satisfy existing ecosystem requirements, apps may require the files to appear to exist in their existing install directory. For example, if a particular app was expecting a file in a folder, like C:\Program Files\Contoso; that directory can be modified by the admins. So starting in Windows 11, apps can specify a directory outside of the WindowsApps directory and the OS will ensure that files will appear in that location with the ACLs inherited from the parent directory. 

## Declaring projection in the manifest 
To enabl this feature, the package will need to declare where to project the files in the package to. Below is an example

```xml
<Package...> 
  <Extensions> 
    <desktop8:Extension Category="windows.MutablePackageDirectories"> 
      <desktop8:MutablePackageDirectories> 
        <desktop8:MutablePackageDirectory target="$(package.volumeroot)\Program Files\<Folder>" Shared=”true”> 
      </desktop8:MutablePackageDirectories> 
    </Extension> 
  </Extensions> 
</Package> 
```
## Considerations for projection
Before using this feature, here are a list of considerations: 

|Considerations  | Description |
|----------|-----------|
|How to install a package with the ability to project?   |Packages have to install through ProvisionPackageForAllUsers API       |
|Who can install it (users or admins)?   |Admin       |
|Where can the files be projected to (locked location, or anywhere at all)?|Anywhere besides %pf%\windowsapps or %pf%\modifiablewindowsapps   |
|What are the ACLs on the projected directory if we create it?|Inherited from parent directory   |
|Can more than one package declare the same directory?|We do not allow more than one related set to declare the same directory. In other words, if there are multiple packages declaring the same directory, they must all be from the same related set.   |
|What about more than one publisher?|No   |
|How are collisions handled?|Packages and/or pre-existing files are merged. Conflicting files are resolved in specified priority order, or package-name alphabetically, if no order specified   |



