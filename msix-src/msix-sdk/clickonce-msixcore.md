---
title: Create a ClickOnce Solution with MSIX Core
description: This article provides a step by step instruction on how to leverage the MSIX Core bootstrapper, which creates an application using ClickOnce that will allow your users to just download a setup.exe and install their MSIX app through the MSIX Core Installer.
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Create a ClickOnce Solution MSIX Core
For those of you who have existing Win32 apps and want to support your customers that have earlier versions of Windows, you can leverage MSIXCore boostrapper which will create an application using ClikcOnce. This will allow your users to download a setup.exe and install the MSIX app through the MSIXCore Installer. 

# Set up - Hosting app on a web server
To get your app ready for bootstrapping with the MSIX Core installer, you’ll need to host your app package on a web server. This article will help you with the specifics of setting up a web app on Azure, Internet Information Services (IIS), or Amazon Web Services (AWS). This article will go through these three seperate options. 
1. Azure 
2. Internet Information Services 
3. Amazon Web Services (AWS)

## Azure 
### Prerequisite
You will need an Azure subscription. To obtain one, visit the [Azure account page](https://azure.microsoft.com/en-us/free/)
### Create an Azure Web App
To get started go to the [Azure portal page](https://portal.azure.com/) and follow the steps below. 
1. Click **Create a Resource**
2. Click **Web** and select **Web App**
3. Under **Instance Details** create a unique app name and select the approperiate settings for your app. For example, you will need to choose between **Code or Docker Container** and the **Runtime Stack**. Otherwise, leave everything else default. 
4. Click **Create** and finish the wizard. 
### Hosting the app package and the web page
Once the web app had been created, click on the app. Under **Development Tools**, click on **App Service Editor**
In the editor, there is a default **hostingstart.html** file. Right-click in the empty space of file explorer and select **Upload Files** to begin uploading your app packages. Right click in the empty space of the file explorer panel and select **New Files** to create a new file. Name the file what you want your default html page to be.
### Configure the web app for app package MIME types
Add a new file to the web app named: Web.config. Open the Web.config file from the explorer and add the following lines.
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
	<system.webServer>
	<!--This is to allow the web server to serve resources with the appropriate file extensions-->
		<staticContent>
			<mimeMap fileExtension=".appx" mimeType="application/appx" />
			<mimeMap fileExtension=".msix" mimeType="application/msix" />
			<mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
			<mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
			<mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
		</staticContent>
	
	</system.webServer>
</configuration>
```



