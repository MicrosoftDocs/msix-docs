---
Description: This article provides all the details you need to manage deploying you MSIX applications in an enteroprise environment.  This article is targeted at enterprise and IT developers.
title: Managing your MSIX deployment
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, deployment, msix
ms.assetid:  
ms.localizationpriority: medium
---

# Managing your MSIX deployment [kevin] byte nme


Packaging your application is only half the battle.  Next you need to be able to deploy your application to your users.  How you deploy it depends on who your customer is.  For enterprise customers, a software management system is used.  If you are targeting retail users, then deployment happens though the web and resources like the Windows Store.
This section will discuss deployment for both enterprise and retail markets.  It will provide links and tips and tricks to ensuring a successful experience.


## Know your client [kevin]

may not support certain API calls your application is built using.   The following table will hope all developers understand their MSIX needs. 
Operating System	Version	Consideration
Windows 10	1909 (10.0.18363.0) or greater 2004 or greater 	MSIX Native Support
Service support
Windows 10	1709 (10.0.16299.0) or greater	MSIX Native Support
Windows 10	10.0.10240.0 – 10.0.15063.0	Must use MSIXCore
Windows 7 / 8		Must use MSIXCore
[Dian] add server stuff in table and update the version. Considerations for deployment – i.e SCCM, intune basically deployment technology and environment 

### MSIX Service support
### MSIX Native support
### MSIXCore

## Creating your MSIX [kevin]

### Visual Studio
Most developers using Visual Studio 2019 <link> today will build their installer using MSIX.  MSIX has been supported natively in Visual studio since build NNNNNN.  Creating a MSIX can be as simple as choosing Publish from the Visual Studio menu.  See Developer nonsense section by Tanaka. <link> 
### Converting your installer
For those that are not writing new code, but instead deploying existing applications, the tool to help you do this is the MSIX Packaging Tool.  With the use of this tool, you can convert your existing MSI or installer to MSIX for installation on the above supported operating systems.  See conversions made easy by someone <link>.
### 	bulk operations - toolkit

##	Distributing through enterprise [roy]
### Microsoft Endpoint Management [roy]
####	Configmanager 
####	Intune
###	Web
###	Updating MSIX
###	Using Group policy [roy]

###	DISM and Provisioning
[Dian] No matter who logs on, the apps is there. Be strict that if a user removes an app, provisioning again does not bring back the app. 1903 override this behavior. 
###	Targeting special groups
###	Known issues
[Dian] Kevin and Dian to talk to Erin from Configmanager 
## Distributing through store
### Microsoft Store
### Microsoft Store for Business
### App Center
#### Known Issues
## Managing your app [kevinla ]
###	prevent app installs through AppLocker [roy]
### Force update
### delay registration
### rollback
### force takedown 
 
## Uninstall [kevinla]
When a package is uninstalled by the user, all files and folders located under *C:\Program Files\WindowsApps\package_name* are removed, as well as any redirected writes to AppData or the registry that were captured during the packaging process.
[Dian] take it off the box. 
## Troubleshooting

### device portal 
### appx logs [kevinla]
[Dian] john to send the name of the name of logs. 
            

# BUGBUG  Kevin and Roy need to add Dian latest notes.

## Next steps

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
