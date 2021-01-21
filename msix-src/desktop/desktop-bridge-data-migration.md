---
description: Guide for smooth user transition and data migration for desktop bridge apps
title: Desktop Bridge - Smooth User Transition & Data Migration 
ms.date: 01/21/2021
ms.topic: article
keywords: windows 10, desktop bridge
ms.localizationpriority: medium
ms.custom: RS5
---

# Desktop Bridge: Smooth User Transition & Data Migration

## Overview

Users will be encouraged to download the store version of their desktop apps. If the user already has the previous desktop version of the app, the transition experience should be as seamless as possible.

Refer to the following [GitHub sample](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

## User Transition: Taskbar Pins & Start Tiles

Many users typically pin their favorite or most used apps to the taskbar pin or the start menu. This enables them to access the app much faster.

Here’s a sample AppXManifest.xml snippet which shows the declaration for this transition.

You must use the following namespace:

```
xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
```

This extension must be under the *<Application>* element.

```
<rescap3:Extension Category="windows.desktopAppMigration">
   <rescap3:DesktopAppMigration>
      <rescap3:DesktopApp AumId="Foo.bar" />
      <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\foo.lnk" />
      <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\foo.lnk" />
      <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\bar\foo.lnk"/>
   </rescap3:DesktopAppMigration>
</rescap3:Extension>
```

If you know your app’s [Application User Model ID](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378459(v=vs.85).aspx), then it’s recommended that you use it for the transition.

Otherwise, you can use the path to your app’s shortcut (.lnk) file. If you don’t know where your app installs the shortcut, the easiest way to find it is to right-click on the app’s name from the start menu and select ‘*Open file location’* .

You have to make sure the path you use in the *AppXManifest.xml* uses the environment variables (one of *%USERPROFILE%* , *%APPDATA%* , or *%PROGRAMDATA%* ). Most likely, the shortcut is under *%PROGRAMDATA%* .

## User Transition: File type associations & protocol handlers

The user may choose their favorite app to be the default app for a given file type or protocol.

Here’s the AppXManifest.xml snippet that shows how the Filetype Association is transitioned. The Protocol Handlers are transition in the same way.

You must use the following namespace:

```
xmlns:uap3=”http://schemas.microsoft.com/appx/manifest/uap/windows10/3”
```

As part of this transition, you need to provide the [Programmatic Identifier](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144152(v=vs.85).aspx).

```
<uap:Extension Category="windows.fileTypeAssociation">
<uap3:FileTypeAssociation Name=“.foo”>
   <rescap3:MigrationProgIds>
      <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
  </rescap3:MigrationProgIds>
 …
 </uap3:FileTypeAssociation>
</uap:Extension>
```

## Data Migration

As best practice, it is recommended for developers to attempt to migrate previous user data from the desktop app, upon first launch of the store version of the same app. This will delight the users, so they can ‘pick up where they left off’.

Please refer to the code sample below to learn about the best practices around how to do this on first launch, while recognizing which version of your desktop app you are running (store version vs previous desktop version).

## User Transition: Uninstall previous desktop app

As best practice, it is recommended for developers to offer uninstallation of the previous desktop app upon first launch of the store version of the app. This will help in avoiding user confusion and potential user data corruption.

Keep in mind that the user can refuse the uninstallation of the previous desktop app, so the previous and store version of the app may end up running side-by-side. It is up to the app developer to decide whether or not to block the launch of the store version of the app, until the previous desktop app is uninstalled.

```
//Please refer to the following blogpost to learn how you can use this method:
//https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/
if (IsRunningAsUwp())
{
   //Detect if there is previous user data
   //In this example, the previous user data was in the ApplicationData You should change this based on where the previous data was

   String sourceDir = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData) + "\\AppName";
   if(Directory.Exists(sourceDir))
   {
      String migrateMessage = "Would you like to migrate your data from the previous version of this app?";
      MessageBoxResult migrateResult = MessageBox.Show(migrateMessage, "Data Migration", MessageBoxButton.YesNo);

      if (migrateResult.Equals(MessageBoxResult.Yes))
      {
         //Migrate user data
         String destinationDir = Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

         //If you are moving data from one of the redirected folders, you need to use robocopy.exe to bypass redirection. This is the only time you should bypass redirection
         //Redirected folders: https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-behind-the-scenes
         //Robocopy.exe: https://technet.microsoft.com/en-us/library/cc733145(v=ws.11
        //Otherwise, you should move files using System.IO

         if (runProcess("robocopy.exe", sourceDir + " " + destinationDir + " /move") > 1 )
         {
            //Migration was unsuccessful -- App Developer can choose to block/retry/other action
         }
      }
   }


   //Detect if the previous version of the Desktop App is installed
   //Typically your uninstall string lives under this

   String uninstallString = (String)Microsoft.Win32.Registry.GetValue(@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

   if (uninstallString != null)
   {
      String uninstallMessage = "It is recommended that you uninstall the old version of this application in order to have a better experience. Would you like to uninstall the previous version of your app now?";
      MessageBoxResult uninstallResult = MessageBox.Show(uninstallMessage, "Uninstall the previous version", MessageBoxButton.YesNo);

      if (uninstallResult.Equals(MessageBoxResult.Yes))
      {
         //Run the uninstaller by using Process (see private method below)
         string[] uninstallArgs = uninstallString.Split(' ');
         if (runProcess(uninstallArgs[0], uninstallArgs[1]) != 0)
         {
            //Uninstallation was unsuccessful - App Developer can choose to block the app here
         }
      }
   }
}


private int runProcess(string appName, string arguments)
{
   Process process = new Process();
   process.StartInfo.FileName = appName;
   process.StartInfo.Arguments = arguments;
   process.StartInfo.CreateNoWindow = true;
   process.Start();
   process.WaitForExit();
   return process.ExitCode;
}
```