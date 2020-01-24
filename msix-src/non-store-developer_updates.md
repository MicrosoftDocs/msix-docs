---
title: Non-store developer controlled updates 
description: Describes how MSIX packages shipped outside the Store can be updated by developers in code. 
author: Huios
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Updating non-Store Distributed apps from your code

When shipping your app as an MSIX you can programmatically kick-off an update of your application. If you're shipping outside the Store, all you need to do is check your server for a new version of your app and install the new version. You can update to a new version by taking advantage of the [PackageManager.UpdatePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.updatepackageasync) API. Doing so requires that your app declares the packageManagement capability. Below is a manifest snipet of of the packageManagement capability declaration and sample C# code of what checking for an update and then using ```PackageManager.UpdatePackageAsync( )``` to install the update can look like:


## Add the PackageManagement Capability to your package manifest

To use the PackageManager APIs your app will need to declare the packageManagement capability in your manifest file. 

```
<Package>
...

 <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
 ...
 </Package>
```
## Check for updates on your server 

The first step is to check your server if ther's a new version of your application available. In this example we check to see of the version of the package on our server is greater than the current version of the app.
```
using Windows.Management.Deployment;

//check for an update on my server
private async void CheckUpdate(object sender, TappedRoutedEventArgs e)
    {
        WebClient client = new WebClient();
        Stream stream = client.OpenRead("https://trial3.azurewebsites.net/HRApp/Version.txt");
        StreamReader reader = new StreamReader(stream);
        var newVersion = new Version(await reader.ReadToEndAsync());
        Package package = Package.Current;
        PackageVersion packageVersion = package.Id.Version;
        var currentVersion = new Version(string.Format("{0}.{1}.{2}.{3}", packageVersion.Major, packageVersion.Minor, packageVersion.Build, packageVersion.Revision));

        //compare package versions
        if (newVersion.CompareTo(currentVersion) > 0)
        {
            var messageDialog = new MessageDialog("Found an update.");
            messageDialog.Commands.Add(new UICommand(
                "Update",
                new UICommandInvokedHandler(this.CommandInvokedHandler)));
            messageDialog.Commands.Add(new UICommand(
                "Close",
                new UICommandInvokedHandler(this.CommandInvokedHandler)));
            messageDialog.DefaultCommandIndex = 0;
            messageDialog.CancelCommandIndex = 1;
            await messageDialog.ShowAsync();
        } else
        {
            var messageDialog = new MessageDialog("Did not find an update.");
            await messageDialog.ShowAsync();
        }
    }
```



## Download an update

Once you've determined that an update is available, you can queue it up. The update will be applied the next time your app is shut down and upon restartng the app the new version will be available to the user.

```
//queue up the update and close the current app instance
    private async void CommandInvokedHandler(IUICommand command)
    {
        if (command.Label == "Update")
        {
            PackageManager packagemanager = new PackageManager();
            await packagemanager.UpdatePackageAsync(
                new Uri("https://trial3.azurewebsites.net/HRApp/HRApp.msix"),
                null,
                DeploymentOptions.ForceApplicationShutdown
            );
        }
    }
```
