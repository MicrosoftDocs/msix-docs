---
title: Store only Developer Package Update 
description: Describes how MSIX packages can be updated by developers in code. 
ms.date: 09/10/2018
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Developer controlled updates for apps in the Store

Starting at Build 14393 and beyond, Windows 10 allows developers to make stronger guarantees around Store app updates.
Doing this requires a few simple APIs, creates a consistent and predictable user experience and lets developers to focus on what they do best while allowing Windows to do the heavy lifting. 

## Implementing in app
There are two fundamental ways that app updates can be managed. In both cases, the net result for these methods is the same - the update is applied. However, in one case, you can choose to let the system do all the work while in the other case you might want to have a deeper level of control over the user experience.

## Simple updates
First and foremost is the very simple API call that tells the system to check for updates, download them and then request permission from the user to install them. You'll start by using the [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) class to get [StorePackageUpdate](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) objects, download and install them. 

```
using Windows.Services.Store;

private async void GetEasyUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation = 
            updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);
        StorePackageUpdateResult result = await downloadOperation.AsTask();
    }
}
```
At this point the user has two options they can choose from; apply now or defer the update. Whatever choice the user makes will be returned back to via the StorePackageUpdateResult object allowing developers to take further actions such as closing down the app if the update is required to continue or simply trying again later.

## Finer controlled updates
For developers who are looking to have a completely customized experience, additional APIs are provided which enable more control over the update process. The platform enables you to do the following:

1. Get progress events on an individual package download or on the whole update
2. Apply updates at the user's and app's convenience rather than one or the other

Developers are able to download updates in the background (while app is in use) then request the user install updates, if they decline, you can simply disable capabilities affected by the update if you choose.

### Download Updates
```
 private async void DownloadUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
            updateManager.RequestDownloadStorePackageUpdatesAsync(updates);

        downloadOperation.Progress = async (asyncInfo, progress) =>
        {
            // Show progress UI
        };

        StorePackageUpdateResult result = await downloadOperation.AsTask();
        if (result.OverallState == StorePackageUpdateState.Completed)
        {
            // Update was downloaded, add logic to request install
        }
    }
}
```

### Install Updates
```
 private async void InstallUpdatesAsync()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();    

    // Save app state here

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        updateManager.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    StorePackageUpdateResult result = await installOperation.AsTask();

    // Under normal circumstances, app will terminate here

    // Handle error cases here using StorePackageUpdateResult from above
}
```

### Making updates mandatory
In some cases, it might actually be desirable to have an update that must be installed to a user's device - making it truly mandatory (e.g. a critical fix to an app that can't wait). In these cases, there are additional measures that you can take to make the update mandatory.
1. Implement the mandatory update logic in your app code (would need to be done before mandatory update itself)
2. During submission to the Dev Center, ensure the "Make this update mandatory" box is selected

### Implementing app code
In order to take full advantage of mandatory updates, you'll need to make some slight modifications to the code above. You'll need to use the [StorePackageUpdate object](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) to determine if the update is mandatory.

```
 private async bool CheckForMandatoryUpdates()
{
    StoreContext updateManager = StoreContext.GetDefault();
    IReadOnlyList<StorePackageUpdate> updates = await updateManager.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        foreach (StorePackageUpdate u in updates)
        {
            if (u.Mandatory)
                return true;
        }
    }
    return false;
}
```
Then you'll need to create a custom in app dialog to inform the user that there is a mandatory update and that they must install it to continue full use of the app. If the user declines the update, the app could either degrade functionality (e.g. prevent online access) or terminate completely (e.g. online only games)

### Partner Center 
To ensure the [StorePackageUpdate](https://docs.microsoft.com/en-us/uwp/api/Windows.Services.Store.StorePackageUpdate) shows true for a mandatory update, you will need to mark the update as mandatory in the Partner Center in the Packages page.

A couple of things to note: 
1. If a device comes back online after a mandatory update has been superseded with another non-mandatory update, the non-mandatory update will still show up on the device as mandatory given the missed update before it was mandatory.
2. Developer controlled updates / mandatory updates is currently limited to the Windows Store.
 
While we don't always like to force updates onto our users or devices, it's sometimes a necessary part of the development life cycle that we can't avoid. Windows 10 makes this process simple, intuitive and manageable for both developers and users. 
