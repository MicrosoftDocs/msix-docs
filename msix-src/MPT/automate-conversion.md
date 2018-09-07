title: 
description:
author: lahugh
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
ms.prod: windows
ms.technology: msix
keywords: windows 10, uwp
ms.localizationpriority: medium

# Automating conversion

MSIX Packaging Tool supports command line interface for creating MSIX application packages which allows the user to automate repackaging and doing bulk conversions with a simple PowerShell script.

Below is a simple powershell script that using a sample conversion template creates corresponding templates for each installer in a folder path and passes the templates to the MSIX packaging tool to create MSIX Packages.


```ps1
$root = "C:\Installers\"
$ConversionTemplate = $root + "\SampleTemplate.xml"
$MSIXSaveLocation = "C:\MSIX\Converted"

get-childitem $root -recurse | where {$_.extension -eq ".msi"} | % {
  
    $Installerpath = $_.FullName
    $filename = $_.BaseName
    $XML_Path = $root + $filename + "ConversionTemplate.xml"

    [xml]$XmlDocument = Get-Content $ConversionTemplate
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Path = $Installerpath
    $XmlDocument.MSIXPackagingToolTemplate.Installer.Arguments = "/qb"
    $XmlDocument.MSIXPackagingToolTemplate.Installer.InstallLocation = "C:\Program Files (x86)\"

    $XmlDocument.MSIXPackagingToolTemplate.SaveLocation.Path = $MSIXSaveLocation 
    
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherName = "CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" 
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PublisherDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.PackageDisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ExecutableName = $filename +".exe"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.DisplayName = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.Description = "$filename"
    $XmlDocument.MSIXPackagingToolTemplate.PackageInformation.Applications.Application.ID = $filename +"1"

    $xmldocument.Save($XML_Path)

    MsixPackagingTool.exe create-package --template $XML_Path

}
```
