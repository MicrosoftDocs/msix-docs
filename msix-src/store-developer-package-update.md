---
title: Update Store-published apps from your code 
description: Describes how MSIX packages can be updated by developers in code. 
author: Huios
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, app package, app update, msix, appx
ms.localizationpriority: medium
ms.custom: "RS5, seodec18"
---

# Update Store-published apps from your code

Starting in Windows 10, version 1607 (build 14393), Windows 10 allows developers to make stronger guarantees around app updates from the Store. Doing this requires a few simple APIs, creates a consistent and predictable user experience and lets developers to focus on what they do best while allowing Windows to do the heavy lifting.

There are two fundamental ways that app updates can be managed. In both cases, the net result for these methods is the same - the update is applied. However, in one case, you can choose to let the system do all the work while in the other case you might want to have a deeper level of control over the user experience.

## Simple updates

First and foremost is the very simple API call that tells the system to check for updates, download them and then request permission from the user to install them. You'll start by using the [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) class to get [StorePackageUpdate](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) objects, download and install them.

```csharp
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

At this point the user has two options they can choose from: apply the update now or defer the update. Whatever choice the user makes will be returned back to via the `StorePackageUpdateResult` object allowing developers to take further actions such as closing down the app if the update is required to continue or simply trying again later.

## Fine-controlled updates

For developers who want to have a completely customized experience, additional APIs are provided which enable more control over the update process. The platform enables you to do the following:

* Get progress events on an individual package download or on the whole update.
* Apply updates at the user's and app's convenience rather than one or the other.

Developers are able to download updates in the background (while app is in use) then request the user install updates, if they decline, you can simply disable capabilities affected by the update if you choose.

### Download updates

```csharp
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

### Install updates

```csharp
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

## Making updates mandatory

In some cases, it might actually be desirable to have an update that must be installed to a user's device - making it truly mandatory (e.g. a critical fix to an app that can't wait). In these cases, there are additional measures that you can take to make the update mandatory.

1. Implement the mandatory update logic in your app code (would need to be done before mandatory update itself).
2. During submission to the Dev Center, ensure the **Make this update mandatory** box is selected.

### Implementing app code

In order to take full advantage of mandatory updates, you'll need to make some slight modifications to the code above. You'll need to use the [StorePackageUpdate object](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) to determine if the update is mandatory.

```csharp
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

Then you'll need to create a custom in app dialog to inform the user that there is a mandatory update and that they must install it to continue full use of the app. If the user declines the update, the app could either degrade functionality (for example, prevent online access) or terminate completely (for example, online-only games).

### Partner Center

To ensure the [StorePackageUpdate](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StorePackageUpdate) shows true for a mandatory update, you will need to mark the update as mandatory in the Partner Center in the **Packages** page.

A couple of things to note:

* If a device comes back online after a mandatory update has been superseded with another non-mandatory update, the non-mandatory update will still show up on the device as mandatory given the missed update before it was mandatory.
* Developer-controlled updates and mandatory updates are currently limited to the Store.
