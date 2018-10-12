---
title: AppInstaller file updates | Microsoft Docs
description: Learn how to update the AppInstaller file
author: laurenhughes
ms.author: lahugh
ms.date: 09/07/2018
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# Appinstaller File 

Often, you need to share your app with many users. Later you need to update the app and you want to make sure you can do that in a way that is seamless even for your non-technical users, and easy for you. 

To help you achieve this, we introduce created the Appinstaller file. This is an XML file, which you can write yourself or use Visual Studio to get you started (see Visual Studio instructions here). The Appinstaller file specifies where your app is located and how to update it. If you choose to use this method of app distribution, you must share with your users the Appinstaller file, instead of the actual app container. The user must then click on the Appinstaller file. At this point the familiar App Installer UI will appear and guide the user through the installation.  Once the user has installed the application using these steps, the application is associated with the appinstaller file.  

Later, when you have an update to the application, you only update the Appinstaller file. When you update the file, the new version of the application is pushed to the user. This is especially good for your users because they don’t have to do anything to get the update. They just keep using the application as usual, and the update will be delivered to them.


Here's an example showing how this works:

1. IT Pro Joe wants to distribute the Human Resources app to his enterprise.
2. IT Pro Joe puts the Human Resources app on a share and creates an Appinstaller file named HumanResources.appinstaller. This Appinstaller file is associated with the app.
3. IT Pro Joe puts HumanResources.appinstaller on a share
4. IT Pro Joe points the enterprise’s employees to HumanResources.appinstaller
5. Manager Maggie clicks on HumanResources.appinstaller and gets the App Installer UI, which guides her to install the Human Resources application.
6. From that point, on manager Maggie’s device Human Resources is just another app and she interacts with it as she does with any other app. She can pin it to the task bar or the start menu, it appears in her apps list etc.
7. A week later IT pro Joe gets an update to the Human Resources app. To share it with users, he just updates HumanResources.appinstaller to point to the new app version and sets the update type he wants. 
8. The next morning, Manager Maggie, who doesn’t know anything about the update launches the Human Resources application that’s already on her desktop. 
9. The application detects that there’s an update and applies the update automatically
10.	Manager Maggie is happy that she now has the latest version of the application and can take advantage of the new features.


To understand how the Appinstaller file manages updates, see the Appinstaller File Overview section.

**Appinstaller File Overview**

As we discussed earlier, the Appinstaller file is an XML file that allows you to distribute and update apps. To make this happen, the Appinstaller file has several sections that we’ll discuss in more detail later. 
 
![ApInstallerFileUpdate](images/App-Installer-File-Update.png)

For details on each section, see the AppInstaller Schema reference
For a summary of what’s new, see the Update Settings below


**Update Settings:**

As mentioned earlier, you can select what kind of update behavior the app should have. Here we explore the update options and their respective trade-offs.

In short, you can choose to check for updates 2 different ways: 1) completely independently of the user launching the app 2) only when the user launches the app; 
Additionally, you can choose to apply updates in 2 different ways: 1) by informing the user with a prompt 2) silently, without informing the user.
Finally, when you inform the user of an update, you can either force them to take the update before allowing them to launch the app, or you can allow them to launch the app and apply the update at an opportune time. 

Specifically, the syntax that is available to you is the following: 

- Update settings “UpdateSettings” can have the following elements: 

    - “OnLaunch”, which checks for updates on launch. This type of update can show UI and has the following attributes:

        - “ShowPrompt” – a Boolean that determines if UI will be shown to the user

        - “UpdateBlocksActivation” – a Boolean that determines if the UI shown to the user allows the user to launch the app without taking the update, or if the user must take the update before launching the app. This attribute can be set to “true” only if “ShowPrompt” is set to “true”. UpdateBlocksActivation =“true” means the UI the user will see allows the user to take the update or close the app. UpdateBlocksActivation =“false” means the UI the user will see allows the user to take the update or start the app without updating. In the latter case, the update will be applied silently the next time the app is launched.

        - “HoursBetweenUpdateChecks” – an integer that indicates how often (in how many hours) the system will check for updates to the app. “0” to “255” inclusive. The default value is 24 (if this value is not specified). For example if HoursBetweenUpdateChecks = 3 then when the user launches the app, if the system has not checked for updates within the past 3 hours, it will check for updates now.  
    
    - “AutomaticBackgroundTask” –  which checks for updates in the background every 8 hours independently of whether the user launched the app. This type of update cannot show UI.
    
    - “ForceUpdateFromAnyVersion” –  which allows the app to update from version x to version x++ or to downgrade from version x to version x--. Without this element, the app can only move to a higher version.
    

<br>
<br>
<div class="container centered pageFooter">
        <h2>Have feedback for us? We'd love to hear it.</h2>
        <ul class="links">
           <li>
                <a href="mailto:MSIXWebsiteFeedback@service.microsoft.com" data-linktype="external">
                    Email the MSIX team
                </a>
            </li>
           
        </ul>
		</div>

<!--
 <div class="container centered pageFooter">
        <h2>Keep in touch with us</h2>
        <ul class="links">
           <li>
                <a href="https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX">
                    MSIX tech community
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/MSIX-PackageSupportFramework/issues">
                    Package Support Framework
                </a>
            </li>
            <li>
                <a href="https://github.com/Microsoft/msix-packaging/issues">
                    MSIX SDK
                </a>
            </li>
            <li>
                <a href="https://twitter.com/#!/search/realtime/%23msix">
                    Twitter
                </a>
            </li>
            
        </ul>
		</div>
-->
