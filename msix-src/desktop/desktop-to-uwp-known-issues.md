---
Description: This article contains known issues with the Desktop Bridge.
title: Known Issues with packaged desktop apps
ms.date: 06/20/2018
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, uwp, msix
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
---
# Known Issues with packaged desktop apps

This article contains known issues that can occur when you create an MSIX package for your desktop app.

## You receive the error	MSB4018	The "GenerateResource" task failed unexpectedly

This can happen when trying to convert satellite assemblies to Package Resource Index (PRI) files.

We are aware of this issue and are working on a more long term solution. As a temporary workaround, you can disable the resource generator by adding this line of XML to the first PropertyGroup element in hosting project file:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## Blue screen with error code 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

After installing or launching certain apps from the Microsoft Store, your machine may unexpectedly reboot with the error: **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**.

Known affected apps include Kodi, JT2Go, Ear Trumpet, Teslagrad, and others.

A [Windows update (Version 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) was released on 10/27/16 that includes important fixes that address this issue. If you encounter this problem, update your machine. If you are not able to update your PC because your machine restarts before you can log in, you should use system restore to recover your system to a point earlier than when you installed one of the affected apps. For information on how to use system restore, see [Recovery options in Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

If updating does not fix the problem or you aren't sure how to recover your PC, please contact [Microsoft Support](https://support.microsoft.com/contactus/).

If you are a developer, you may want to prevent the installation of your packaged application on versions of Windows that do not include this update. Note that by doing this your application will not be available to users that have not yet installed the update. To limit the availability of your application to users that have installed this update, modify your AppxManifest.xml file as follows:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Details regarding the Windows Update can be found at:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## Common errors that can appear when you sign your app

### Publisher and cert mismatch causes Signtool error "Error: SignerSign() Failed" (-2147024885/0x8007000b)

The Publisher entry in the Windows app package manifest must match the Subject of the certificate you are signing with.  You can use any of the following methods to view the subject of the cert.

**Option 1: Powershell**

Run the following PowerShell command. Either .cer or .pfx can be used as the certificate file, as they have the same publisher information.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Option 2: File Explorer**

Double-click the certificate in File Explorer, select the *Details* tab, and then the *Subject* field in the list. You can then copy the contents.

**Option 3: CertUtil**

Run **certutil** from the command line on the PFX file and copy the *Subject* field from the output.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### Bad PE certificate (0x800700C1)

This can happen when your package contains a binary that has a corrupted certificate. Here's some of the reasons why this can happen:

* The start of the certificate is not at the end of an image.  

* The size of the certificate isn't positive.

* The certificate start isn't after the `IMAGE_NT_HEADERS32` structure for a 32-bit executable or after the `IMAGE_NT_HEADERS64` structure for a 64-bit executable.

* The certificate pointer isn't properly aligned for a WIN_CERTIFICATE structure.

To find files that contain a bad PE cert, open a **Command Prompt**, and set the environment variable named `APPXSIP_LOG` to a value of 1.

```
set APPXSIP_LOG=1
```

Then, from the **Command Prompt**, sign your application again. For example:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Information about files that contain a bad PE cert will appear in the **Console Window**. For example:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## Next Steps

**Find answers to your questions**

Have questions? Ask us on Stack Overflow. Our team monitors these [tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). You can also ask us [here](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Give feedback or make feature suggestions**

See [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
