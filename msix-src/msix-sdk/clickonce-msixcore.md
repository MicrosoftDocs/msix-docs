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
Now you are done hosting your web app on Azure. 

## Internet Information Services (IIS)
### Prerequisite
**Internet Information Services** is a Windows feature that can be installed via the **Start** menu. In **Start** menu search for **Turn Windows features on or off**. Find and select **Internet Information Services**. You will also need to install **ASP.NET 4.5** or greater. To install it, locate **Internet Information Services** -> **World Wide Web Services** -> **Application Development Features**. Select a version of **ASP.NET** that is greater than or equal to **ASP.NET 4.5**.

Installing Visual Studio 2017 (or greater) and Web Development Tools are required. If you already have VS 2017, ensure that you have the following workloads: ASP.NET and Web development. Otherwise, [install Visual Studio 2017 from here](https://docs.microsoft.com/visualstudio/install/install-visual-studio). 

### Build a web app
Launch Visual Studios as **Administrator** and create a new **Visual C# Web Application** project with an empty project template.

### Configure IIS with our Web App
From the Solution Explorer, right click on the root project and select **Properties**. In the web app properties, select the **Web** tab. In the **Servers** section, choose **Local IIS** from the dropdown menu and click **Create Virtual Directory**.

### Add an app package to a web application
Add the app package that you are going to distribute into the web application. To create the folder in Visual Studio, right click on the project node in **Solution Explorer**, select* **Add** -> **New Folder** and name it **packages**. To add app packages to the folder, right click on the packages folder and select **Add** -> **Existing Item** and browse to the app package location.

### Create a Web Page 
Create an HTML page or any other web app as required per your needs. Just add the link of your new setup.exe.

### Configure the web app for app package MIME types
Open the **Web.config** file from the solution explorer and add the following lines within the <configuration> element.
```xml
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
```
Now you are done with setting up your web app with IIS

##  Amazon Web Services (AWS)
### Prerequisite
You will need to have an AWS membership. For more information visit, [AWS account details](https://aws.amazon.com/free/)
### Create an Amazon S3 bucket and Upload your MSIX Package(s) and web page(s)
Amazon Simple Storage Service (S3) is an AWS offering for collecting, storing and analyzing data. S3 buckets are a convenient way to host Windows 10 app packages and web pages for distribution.

After logging in to AWS with your credentials, under Services find S3.

Select **Create bucket**, and enter a **Bucket name** for your website. Follow the dialog prompts for setting properties and permissions. To ensure that your Windows 10 app can be distributed from your website, enable **Read** and **Write** permissions for your bucket and select Grant public read access to this bucket. Click Create bucket to finish this step.

When you are finished, upload your MSIX package(s) and web page(s) to an S3 Bucket 

### Configure the web app for app package MIME types
Navigate to **Tools** and select **Default HTTP Headers.** Default HTTP Headers dialog will open, and click the Add button. An Add New Default HTTP Headers dialog will appear. Specify the Bucket, File Name, Header name, and Header Value, and click **Add new header.**
```
**Bucket name**: msix-packages
**File name**: *.msix
**Header name**: Content-Type
**Header value**: application/msix
```


