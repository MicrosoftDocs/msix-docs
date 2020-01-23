---
title: Non-store developer controlled updates 
description: Describes how MSIX packages shipped outside the Store can be updated by developers in code. 
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Updating non-Store Distributed apps from your code

Windows 10 allows you to programmatically check for new versions of your MSIX packages distributed outside the Store. You can do so by taking advantage of the [PackageManager.UpdatePackageAsync()] API(https://docs.microsoft.com/en-us/uwp/api/windows.management.deployment.packagemanager.updatepackageasync). Doing so requires that your app declares the packageManagement capability. Below is some sample C# code.

## Check for updates on your server 

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
Once you have determined that an update is available, you can queue it up. 


### Download an update
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

### Add the PackageManagement Capability to your package manifest
```
<Package>
...

 <Capabilities>
    <rescap:Capability Name="packageManagement" />
  </Capabilities>
  
 ...
 </Package>
```

