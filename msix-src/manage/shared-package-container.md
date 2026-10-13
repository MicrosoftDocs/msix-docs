---
description: This guide explains MSIX Shared Package Container
title: MSIX Shared Package Container
ms.date: 08/27/2026
ms.topic: article
keywords: windows 10, uwp, msix
---

# Shared package container

A shared package container lets multiple MSIX packages run with a merged view of their virtual file systems and virtual registries. An application in the container can therefore resolve package files and virtualized registry state contributed by another package in the same container.

This Windows 11 feature is intended for enterprise management and requires administrator privileges to configure. Common scenarios include:

- A converted application and an add-on that expects to find the application's files or registry state.
- A suite split into separate packages that share a packaged runtime or prerequisite.
- A customization package that overlays files or virtual registry settings for another packaged application.

### Merged view and conflict priority

At runtime, Windows composes the virtual file system and virtual registry views from the installed packages listed in the container definition. The package families are listed in priority order, from highest to lowest. If multiple packages contribute an item at the same virtual path or registry location, the item from the higher-priority package is visible in the merged view. The lower-priority package isn't modified.

Only installed members contribute content to the runtime view. You can deploy a container definition before its packages, and the container can operate when only a subset of its defined packages is installed.

### What a shared package container doesn't do

A shared package container is a targeted compatibility mechanism, not a general removal of MSIX isolation. It doesn't:

- Give an application unrestricted access to the host file system or registry outside the merged virtualized views.
- Merge package identities, manifests, declarations, installation, or servicing. Each package retains its own identity and can be installed, updated, or removed independently. Processes running in the shared package container can, however, share the container's virtualized file and registry state.
- Add package capabilities or supported application extensions that aren't declared in a package manifest.
- Guarantee that applications with other compatibility problems will work after packaging. For example, code that depends on a driver, service, architecture, or integration point that MSIX doesn't support must still be addressed separately.

For a given user, a package family can belong to only one shared package container. Deploying another definition that assigns the same package family to a different container for that user fails.

## Prerequisite  

Use Windows 11 and run the management commands with administrator privileges. Each main package in the definition must be an MSIX package. To convert an existing installer, see [Create an MSIX package from any desktop installer](../packaging-tool/create-an-msix-overview.md).

## Shared package container definition

A shared package container is defined by an XML file. The definition requires a unique name and an ordered list of package family names. Include only main packages. Optional packages and modification packages are included automatically because they already share the container of their main package. The first package family in the list has the highest priority.

```xml
<?xml version="1.0" encoding="utf-8"?> 
<AppSharedPackageContainer Name="ContosoContainer"> 
  <PackageFamily Name="Fabrikam.MainApp_8wekyb3d8bbwe"/> 
  <PackageFamily Name="Contoso.MainApp_8wekyb3d8bbwe"/> 
  <PackageFamily Name="ContosoCustomize_7xekyb3d8ccde"/> 
</AppSharedPackageContainer>   
```
After you create the container definition, use the following PowerShell commands to deploy, inspect, reset, update, or remove the shared package container. These operations are separate from MSIX package deployment. Changing package installation or update state doesn't require changing the container definition.

### PowerShell commands 

#### Deploy a shared package container definition

```powershell
Add-AppSharedPackageContainer <path> 
``` 
This command deploys the shared package container definition for the current user. Optional parameters include the following:

|**Parameter** |	**Description**|
|---------|---------|
|ForceApplicationShutdown |Closes all applications currently running in the shared package container. |

#### Remove a shared package container

```powershell
Remove-AppSharedPackageContainer -Name <name>  
``` 
This command removes the shared package container definition for the current user. Optional parameters include the following:

|**Parameter** |	**Description**|
|---------|---------|
|ForceApplicationShutdown  |Closes all applications in the shared package container.  |

#### Get information on a shared package container
 
```powershell
Get-AppSharedPackageContainer -Name <name> 
``` 
This command gets information about the shared package container. In particular, it will show what packages are inside the shared package container. 

#### Reset shared package container

```powershell
Reset-AppSharedPackageContainer -Name <name>  
``` 
This command deletes the shared package container's application data, including its virtual files and registry keys. Back up required data before running the command.

#### Deploy a provisioned package container

This command deploys a provisioned shared package container.

```powershell
Add-AppProvisionedSharedPackageContainer -DefinitionFile "<filepath>" -Online
```

#### Verify that a provisioned package container is deployed

This command verifies that a provisioned shared package container is deployed

```powershell
Get-AppProvisionedSharedPackageContainer -Online
```

#### Remove a provisioned package container

This command removes a provisioned shared package container

```powershell
Remove-AppProvisionedSharedPackageContainer -Name "<name>" -Online
```
