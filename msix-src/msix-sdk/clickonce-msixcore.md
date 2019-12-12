---
title: Create a ClickOnce Solution with MSIX Core
description: This article provides a step by step instruction on how to leverage the MSIX Core bootstrapper, which creates an application using ClickOnce that will allow your users to just download a setup.exe and install their MSIX app through the MSIX Core Installer.
ms.date: 11/15/2019
ms.topic: article
keywords: windows 10, windows 7, windows 8, Windows Server, uwp, msix, msixcore, 1709, 1703, 1607, 1511, 1507
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Create a ClickOnce solution with MSIX Core

If you have existing desktop apps and want to support your customers that are using Windows 10, version 1703 or earlier versions, you can leverage the MSIX Core installer to create an application using ClickOnce. This will allow your users to download a setup.exe and install the MSIX app through the MSIX Core installer.

## Host your app on a web server

To get your app ready for bootstrapping with the MSIX Core installer, you’ll need to host your app package on a web server. This section provides details about how to set up a web app on [Azure](#azure), [Internet Information Services (IIS)](#internet-information-services-iis), and [Amazon Web Services (AWS)](#amazon-web-services-aws).

### Azure

To use this option you must have an Azure subscription. To obtain one, see the [Azure account page](https://azure.microsoft.com/free/).

#### Create an Azure Web App

To get started go to the [Azure portal page](https://portal.azure.com/) and follow these steps:

1. Click **Create a Resource**.
2. Click **Web** and select **Web App**.
3. Under **Instance Details**, create a unique app name and select the appropriate settings for your app. For example, you will need to choose between **Code or Docker Container** and the **Runtime Stack**. Otherwise, leave everything else default.
4. Click **Create** and finish the wizard.

#### Host the app package and the web page

1. After you create the web app, select the app.
2. Under **Development Tools**, click **App Service Editor**.
3. In the editor, there is a default **hostingstart.html** file. Right-click in the empty space of File Explorer and select **Upload Files** to begin uploading your app packages.
4. Right-click in the empty space of the File Explorer panel again and select **New Files** to create a new file. Name the file what you want your default HTML page to be.

#### Configure the web app for app package MIME types

Add a new file named Web.config to the web app. Open the Web.config file and add the following XML to the file.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
	<system.webServer>
	<!--This is to allow the web server to serve resources with the appropriate file extensions-->
		<staticContent>
			<mimeMap fileExtension=".appx" mimeType="application/appx" />
			<mimeMap fileExtension=".msix" mimeType="application/msix" />
		</staticContent>
	
	</system.webServer>
</configuration>
```

### Internet Information Services (IIS)

IIS is an optional Windows feature. To install IIS:

1. Click **Start** and search for **Turn Windows features on or off**.
2. Select **Internet Information Services**. 
3. Also make sure you install **ASP.NET 4.5** or greater. In the **Windows Features** dialog, expand **Internet Information Services** -> **World Wide Web Services** -> **Application Development Features**, and select a version of **ASP.NET** that is greater than or equal to **ASP.NET 4.5**.
4. Click **OK** to begin the installation.

Visual Studio 2017 (or a later version) and Web Development Tools are required. If you already have Visual Studio 2017 or a later version installed, make sure you have the ASP.NET and Web development workloads installed. Otherwise, install Visual Studio from [here](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

#### Build a web app

Start Visual Studio as an administrator and create a new **Visual C# Web Application** project with an empty project template.

#### Configure IIS with your Web app

1. In **Solution Explorer**, right-click on the root project and select **Properties**. 
2. In properties, select the **Web** tab. 
3. In the **Servers** section, choose **Local IIS** from the dropdown menu and click **Create Virtual Directory**.

#### Add the app package to the web application

Add the app package that you want to distribute to the web application:

1. In **Solution Explorer**, right-click the project node.
2. Select **Add** -> **New Folder** and name the folder **packages**. 
3. To add app packages to the folder, right-click the packages folder and select **Add** -> **Existing Item**. Browse to the app package location.

#### Create a Web Page

Create an HTML page or any other web app as required per your needs. Add the link of your new setup.exe.

#### Configure the web app for app package MIME types

Open the **Web.config** file from the solution explorer and add the following XML within the <configuration> element.

```xml
<system.webServer>
	<!--This is to allow the web server to serve resources with the appropriate file extensions-->
	<staticContent>
		<mimeMap fileExtension=".appx" mimeType="application/appx" />
		<mimeMap fileExtension=".msix" mimeType="application/msix" />
	</staticContent>
</system.webServer>
```

### Amazon Web Services (AWS)

To use this option you must have an AWS membership. For more information, see [AWS account details](https://aws.amazon.com/free/).

#### Create an Amazon S3 bucket and upload your MSIX packages and web pages

Amazon Simple Storage Service (S3) is an AWS offering for collecting, storing and analyzing data. S3 buckets are a convenient way to host Windows 10 app packages and web pages for distribution.

1. Log in to AWS. Under **Services** find **S3**.
2. Select **Create bucket** and enter a **Bucket name** for your website. Follow the dialog prompts for setting properties and permissions. To ensure that your Windows 10 app can be distributed from your website, enable **Read** and **Write** permissions for your bucket and select **Grant public read access to this bucket**. Click **Create bucket** to finish this step.
3. When you are finished, upload your MSIX packages and web pages to the S3 bucket.

#### Configure the web app for app package MIME types

Using a web service interface like [S3 browser](https://s3browser.com/features-content-mime-types-editor.aspx) to add a new **Default HTTP Headers**. 
1. Navigate to **Tools** and select **Default HTTP Headers**.
2. In the **Default HTTP Headers** dialog, click **Add**.
3. In the **Add New Default HTTP Headers** dialog, specify the bucket name, file name, header name, and header value, and then click **Add new header**.
    * **Bucket name**: msix-packages
    * **File name**: *.msix
    * **Header name**: Content-Type
    * **Header value**: application/msix

> [!NOTE]
> AWS have some strict guidelines you will have to follow. For example, Bucket names are required to be unique and therefore if you are using the example above, you will need to change the Bucket name. 

## Use the MSIX Core installer to build the ClickOnce application

Find your application application ClickOnce setup.exe. For an example, you can download the the ClickOnce setup.exe provided by MSIX Core from [https://appinstallerdemo.azurewebsites.net/MSIXCore/setup.exe](https://appinstallerdemo.azurewebsites.net/MSIXCore/setup.exe) from our demo. Note the current source code can be found on GitHub.

### Run URL command to create new setup.exe
#### Prerequisite 
Make sure you have followed the instructions to clone, build and publish the MSIX Core solution in Visual Studios. 

Navigate to the directory where you downloaded the setup.exe file and then run this command: `setup-exe - url=<location of your msix in the webservice>`.

### Sign the application

Because the previous step created a new setup.exe, you will need to sign the app again to verify that you're a trusted publisher of the application and to establish the integrity of the application. You can use the [SignTool](https://docs.microsoft.com/dotnet/framework/tools/signtool-exe) and provide your certificate.

### Distribute the application to your users

You can now point to the new setup.exe with a link or download button on their website. MSIX Core is targeted towards users on Windows 10, version 1703 and earlier. The [App Installer](https://docs.microsoft.com/windows/msix/app-installer/installing-windows10-apps-web) is the ideal installation process for MSIX packages on Windows 1709 or a later version. App Installer optimizes for disk space on the consumer side and can directly install apps from HTTP locations. MSIX Core will detect if a consumer is on Windows 1709 or a later version and redirect them to App Installer. 


On Microsoft Edge, you can call the [getHostEnvironmentValue()](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/mt795399%28v%3dvs.85%29) method and the *os-build* field in the return value will specify the OS version of the user. From there, you can then prompt the installation process to use MSIX Core (for Windows 10, version 1703 and earlier) or App Installer (for Windows 10, version 1709 and later).

## User experience

Users simply download and run the setup.exe from the developer’s webpage.

* If the MSIX Core installer is not yet installed when the user runs setup.exe, the user sees the ClickOnce prompt and they click **Install** to install the MSIX Core installer. The installer automatically launches and shows the install screen for the MSIX package specified in the developer’s query string so the users can install the app.
* If the MSIX Core installer is already installed when the user runs setup.exe, the MSIX Core installer automatically launches and shows the install screen for the MSIX package specified in the query string for users to install the app.