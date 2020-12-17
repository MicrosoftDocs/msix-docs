---
title: Passing installation parameters to your app via App Installer
description: Describes how to pass installation parameters to an app via App Installer and protocol activation.
author: Huios
ms.date: 12/17/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Passing installation parameters to your app via App Installer

When deploying your app as an MSIX via the web, you can pass installation parameters to your MSIX packaged application after it is installed. 
This article shows how you can use configure your MSIX packaged app to receive installation parameters, and setting up your installation uri with the parameters you need to pacc into your app. For more details see this [blog post](https://techcommunity.microsoft.com/t5/windows-dev-appconsult/passing-installation-parameters-to-a-windows-application-with/ba-p/1719829).
## Configure your application for protocol activation

This enables your app to be launched using your custom defined protocol. When your packaged application is started by using a protocol, specified parameters can be passed to its activation event arguments so it can use the arguments when launched. 

```xml
<Application>
...
   </Extensions>
     <uap:Extension Category="windows.protocol">
        <uap:Protocol Name="my-custom-protocol"/>
     </uap:Extension>
   </Extensions>
  
...
</Application>
```

##  Write code to handle parameters when app is launched

You will need to implement code in your application to handle the installation parameters that will be passed to your app at first launch. The example code below uses the [AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs?view=winrt-19041) method to determine the type of activation used to instantiate an app. When your app is launched/activated with install parameters from an install uri as defined in the next section, the activation type will be a protocol activation as defined by your custom protocol in your install uri. So the activation event args will be of type [ProtocolActivatedEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs?view=winrt-19041).

```csharp

using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;

public static void Main(string[] cmdArgs)
        {
            
    var activationArgs = AppInstance.GetActivatedEventArgs();
    switch (activationArgs.Kind)
    {
        //Install parameters will be passed in during a protocol activation
        case ActivationKind.Protocol:
        HandleProtocolActivation(activationArgs as ProtocolActivatedEventArgs);
        break;
        case ActivationKind.Launch:
        //Regular launch activation type
        HandleLaunch(activationArgs as LaunchActivatedEventArgs);
        break;
        default:
        break;
     }       
    

     static void HandleProtocolActivation(ProtocolActivatedEventArgs args)
     {

         if (args.Uri != null)
        {
            //Handle the installation parameters in the protocol uri
            handleInstallParameter(args.Uri.ToString());

        }
            
}
```

## Add your app activation protocol and parameters to the installation uri

Once your app is set up to handle your installation parameters, you can define specific parameters in your [app download uri](https://docs.microsoft.com/en-us/windows/msix/app-installer/installing-windows10-apps-web#protocol-activation-scheme) on your webpage and the parameters you specify will be passed to your app at first launch. In the example uri below, I have defined the a custom protocol _my-custom-protocol_, a parameter _my-parameter_ and given it the value _my-param-value_.

```html
ms-appinstaller:?source=https://contosomsix.com/contosomsix.appinstaller&activationUri=my-custom-protocol:?my-parameter=my-param-value
```
