---
title: How to create a custom App Installer UX
description: A document describing how to create the custom UX xml file and how to add it to your MSIX package to create a custom look and feel for your App Installer installs.
author: sharlaakers
ms.author: shakers # Microsoft employees only
ms.date: 9/30/2021
ms.topic: article
---

# How to create a custom App Installer experience

The App Installer App is used for all MSIX installations providing a consistent experience for all users installing an MSIX application. While this consistency is good, we want to also provide the ability for developers to customize the install experience that they are providing to their users. This feature is available on Windows 10 1709 and later.

## Create your custom MSIXAppInstallerData.xml file

The first thing you are going to need to customize your App Installer experience is the customization xml file. You can customize several features of your App Installer UX, to make your own unique installer experience. Be sure to save the file name as **MSIXAppInstallerData.xml**

Here is a list of parameters available for customization:

| App Installer UX Setting | Description |
| --- | --- |
| UX::AccentColor | A hex code to change the accent color of App Installer |
| UX::FontFamily | Font family |
| UX::AllowUserInteraction | Boolean. If true, the user can see the &#39;launch when ready&#39; checkbox (checked by default) and has the option to cancel the install |
| UX::BackgroundColor | A hex code to change the background color of App Installer |
| UX::AppNameInTitle | Boolean. If true, the app name will appear in the installer window title. |
| HyperLinkFontSize | Hyper link font size. |
| Icon::HorizontalAlignment | Icon alignment within the window. Left, center, right |
| Icon::Logo | Link to icon location |
| Icon::TopMarging | Margin from the top of the icon to the top of the application window. |
| Buttons::HorizontalAlignment | Button alignment within the window. Left, center, right |
| Buttons::Text | Additional text to add to the Install buttonIs |
| Buttons::IsSecondaryButtonAccent | Boolean. |
| LaunchWhenReady::HorizontalAlignment | Alignment of the checkbox for &#39;Launch when ready&#39;. Center, left. |
| AppInformation::Mode | Additional Information show type. Normal, flyout |
| Hyperlinks::TopMargin | Specifies margin between hyperlink and buttons. |
| Hyperlink::Text | Text to display as hyperlink |
| Hyperlink::Url | Link |
| Hyperlink:: HorizontalAlignment | Alignment of hyperlink within the window. Left, center, right |

## Sample xml:
```xml

<?xml version="1.0" encoding="utf-8"?> 

<AppInstallerUX xmlns="http://schemas.microsoft.com/msix/appinstallerux"  

xmlns:ux="http://schemas.microsoft.com/msix/appinstallerux" 

xmlns:ux2="http://schemas.microsoft.com/msix/appinstallerux/2" 

IgnorableNamespaces="ux ux2" Version="1.0.0"> 

  <UX AccentColor="#DE781F" FontFamily="Segoe UI" AllowUserInteraction="false" BackgroundColor="#F3F3F3"  

  AppNameInTitle="true"  

  HyperLinkFontSize="12"> 

    <Icon HorizontalAlignment="center" Logo="Images\Contoso96x96.png" TopMargin="70"/> 

    <Buttons HorizontalAlignment="center" Text="Contoso" IsSecondaryButtonAccent="false"/> 

    <LaunchWhenReady HorizontalAlignment="center"/> 

    <AppInformation Mode="flyout" /> 

    <HyperLinks TopMargin="30"> 

      <HyperLink  Text="Terms &amp; conditions" Url="https://support.microsoft.com/" HorizontalAlignment="center"/> 

    </HyperLinks> 

  </UX> 

</AppInstallerUX> 

```

Save your file as &#39;MsixAppInstallerData.xml&#39;

## Add the xml file to your MSIX application

### Using the MSIX Packaging Tool – Package Editor

1) Open your MSIX application with Package Editor in the MSIX Packaging Tool

2) Go to your Package Files and add a new folder under your Package root called &#39;Msix.AppInstaller.Data&#39;

3) Add your MSIXAppInstallerData.xml file to your newly created folder.

4) Save your MSIX Package – be sure to increment the version and sign the package

## Troubleshooting

- The file must be named MSIXAppInstallerData.xml
- The file must be in the folder named MSIXAppInstallerData
- The folder must be underneath the Package root of the package files
- Check your OS version and your App Installer version
- Double check the validity of your xml file

File feedback if you run into any other problems or reach out to the MSIX team on our Tech Community.
