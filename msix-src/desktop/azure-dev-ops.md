---
description: You can use Azure Pipelines to create automated builds for your MSIX project.
title: Set up a CI/CD pipeline to automate your MSIX builds and deployments
ms.date: 01/27/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
---

# Set up a CI/CD pipeline to automate your MSIX builds and deployments

You can use Azure Pipelines to create automated builds for your MSIX project. This article takes a look at how to do so in Azure DevOps. We’ll also show you how to perform these tasks by using the command line so that you can integrate with any other build system.

## Create a new Azure Pipeline

Begin by [signing up for Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) if you haven't done so already.

Next, create a pipeline that you can use to build your source code. For a tutorial about building a pipeline to build a GitHub repository, see [Create your first pipeline](/azure/devops/pipelines/get-started-yaml). Azure Pipelines supports the repository types listed [in this article](/azure/devops/pipelines/repos).

To set up the actual build pipeline, you browse to the Azure DevOps portal at dev.azure.com/\<organization\> and create a new project. If you don’t have an account, you can create one for free. Once you’ve signed in and created a project, you can either push the source code to the Git repository that’s set up for you at https://\<organization\>@dev.azure.com/<organization\>/\<project\>/_git/\<project\>, or use any other provider, such as GitHub. You’ll get to choose the location of your repository when you create a new pipeline in the portal by clicking first on the **Pipelines** button and then on **New Pipeline**.

On the **Configure** screen that comes next, you should select the **Existing Azure Pipelines YAML file** option and select the path to the checked-in YAML file in your repository.

## Add your project certificate to the Secure files library

> [!NOTE]
>You should avoid submitting certificates to your repo if at all possible, and git ignores them by default. To manage the safe handling of sensitive files like certificates, Azure DevOps supports the [secure files](/azure/devops/pipelines/library/secure-files?view=azure-devops) feature.

To upload a certificate for your automated build:

1. In Azure Pipelines, expand **Pipelines** in the navigation pane and click **Library**.
2. Click the **Secure files** tab and then click **+ Secure file**.
3. Browse to the certificate file and click **OK**.
4. After you upload the certificate, select it to view its properties. Under **Pipeline permissions**, enable the **Authorize for use in all pipelines** toggle.
5. If the private key in the certificate has a password, we recommend that you store your password in [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) and then link the password to a [variable group](/azure/devops/pipelines/library/variable-groups). You can use the variable to access the password from the pipeline. Note that a password is only supported for the private key; using a certificate file that is itself password-protected is not currently supported.

> [!NOTE]
> Starting in Visual Studio 2019, a temporary certificate is no longer generated in MSIX projects. To create or export certificates, use the PowerShell cmdlets described in [this article](../package/create-certificate-package-signing.md).


## Configure the Build in your YAML file

The table below lists the different MSBuild arguments you can define to setup your build pipeline.

|**MSBuild argument**|**Value**|**Description**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Defines the folder to store the generated artifacts. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Enables you to define the platforms to include in the bundle. |
| AppxBundle | Always | Creates an .msixbundle/.appxbundle with the .msix/.appx files for the platform specified. |
| UapAppxPackageBuildMode | StoreUpload | Generates the .msixupload/.appxupload file and the **_Test** folder for sideloading. |
| UapAppxPackageBuildMode | CI | Generates the .msixupload/.appxupload file only. |
| UapAppxPackageBuildMode | SideloadOnly | Generates the **_Test** folder for sideloading only. |
| AppxPackageSigningEnabled | true | Enables package signing. |
| PackageCertificateThumbprint | Certificate Thumbprint | This value **must** match the thumbprint in the signing certificate, or be an empty string. |
| PackageCertificateKeyFile | Path | The path to the certificate to use. This is retrieved from the secure file metadata. |
| PackageCertificatePassword | Password | The password for the private key in the certificate. We recommend that you store your password in [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) and link the password to [variable group](/azure/devops/pipelines/library/variable-groups). You can pass the variable to this argument. |



Before building the packaging project the same way the wizard in Visual Studio does using the MSBuild command line, the build process can version the MSIX package that’s being produced by editing the Version attribute of the Package element in the Package.appxmanifest file. In Azure Pipelines, this can be achieved by using an expression for setting a counter variable that gets incremented for every build, and a PowerShell script that uses the System.Xml.Linq.XDocument class in .NET to change the value of the attribute. 

Sample YAML File that defines the MSIX Build Pipeline

```yml
pool: 
  vmImage: windows-2019
  
variables:
  buildPlatform: 'x86'
  buildConfiguration: 'release'
  major: 1
  minor: 0
  build: 0
  revision: $[counter('rev', 0)]
  
steps:
 - powershell: |
     # Update appxmanifest. This must be done before the build.
     [xml]$manifest= get-content ".\Msix\Package.appxmanifest"
     $manifest.Package.Identity.Version = "$(major).$(minor).$(build).$(revision)"    
     $manifest.save("Msix/Package.appxmanifest")
  displayName: 'Version Package Manifest'
  
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp
     /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:AppxPackageOutput=$(Build.ArtifactStagingDirectory)\MsixDesktopApp.msix /p:AppxPackageSigningEnabled=false'
  displayName: 'Package the App'
  
- task: DownloadSecureFile@1
  inputs:
    secureFile: 'certificate.pfx'
  displayName: 'Download Secure PFX File'
  
- script: '"C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x86\signtool"
    sign /fd SHA256 /f $(Agent.TempDirectory)/certificate.pfx /p secret $(
    Build.ArtifactStagingDirectory)/MsixDesktopApp.msix'
  displayName: 'Sign MSIX Package'
  
- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
```




Below are breakdowns of the different Build tasks defined in the YAMl file:

### Configure package generation properties

The definition below sets the directory of Build components, the platform and defines whether to build a bundle or not. 

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\"
/p:UapAppxPackageBuildMode=SideLoadOnly
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Never
```

### Configure package signing

To sign the MSIX (or APPX) package the pipeline needs to retrieve the signing certificate. To do this, add a DownloadSecureFile task prior to the VSBuild task.
This will give you access to the signing certificate via ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Next, update the MSBuild task to reference the signing certificate:

```yml
- task: MSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Never 
                  p:UapAppxPackageBuildMode=SideLoadOnly 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> The PackageCertificateThumbprint argument is intentionally set to an empty string as a precaution. If the thumbprint is set in the project but does not match the signing certificate, the build will fail with the error: `Certificate does not match supplied signing thumbprint`.

### Review parameters

The parameters defined with the `$()` syntax are variables defined in the build definition, and will change in other build systems.


To view all predefined variables, see [Predefined build variables](/azure/devops/pipelines/build/variables).

## Configure the Publish Build Artifacts task

The default MSIX pipeline does not save the generated artifacts. To add the publish capabilities to your YAML definition, add the following tasks.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

You can see the generated artifacts in the **Artifacts** option of the build results page.


## AppInstaller file for non-store distribution


If you're distributing your application outside the Store you can take advantage of the AppInstaller file for your package install and updates

An .appinstaller File That Will Look for Updated Files on \\server\foo

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller xmlns="http://schemas.microsoft.com/appx/appinstaller/2018"
              Version="1.0.0.0"
              Uri="\\server\foo\MsixDesktopApp.appinstaller">
  <MainPackage Name="MyCompany.MySampleApp"
               Publisher="CN=MyCompany, O=MyCompany, L=Stockholm, S=N/A, C=Sweden"
               Version="1.0.0.0"
               Uri="\\server\foo\MsixDesktopApp.msix"
               ProcessorArchitecture="x86"/>
  <UpdateSettings>
    <OnLaunch HoursBetweenUpdateChecks="0" />
  </UpdateSettings>
</AppInstaller>
```

The UpdateSettings element is used to tell the system when to check for updates and whether to force the user to update. The full schema reference, including the supported namespaces for each version of Windows 10, can be found in the docs at bit.ly/2TGWnCR.

If you add the .appinstaller file to the packaging project and set its Package Action property to Content and the Copy to Output Directory property to Copy if newer, you can then add another PowerShell task to the YAML file that updates the Version attributes of the root and MainPackage elements and saves the updated file to the staging directory:

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $doc = [System.Xml.Linq.XDocument]::Load(
    "$(Build.SourcesDirectory)/Msix/Package.appinstaller")
  $version = "$(major).$(minor).$(build).$(revision)"
  $doc.Root.Attribute("Version").Value = $version;
  $xName =
    [System.Xml.Linq.XName]
      "{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Version").Value = $version;
  $doc.Save("$(Build.ArtifactStagingDirectory)/MsixDesktopApp.appinstaller")
displayName: 'Version App Installer File'
```

You’d then distribute the .appinstaller file to your end users and let them double-click on this one instead of the .msix file to install the packaged app.

## Continuous Deployment

The app installer file itself is an uncompiled XML file that can be edited after the build, if required. This makes it easy to use when you deploy your software to multiple environments and when you want to separate the build pipeline from the release process.

If you create a release pipeline in the Azure Portal using the “Empty job” template and use the recently set up build pipeline as the source of the artifact to be deployed, you can then add the PowerShell task to the release stage in order to dynamically change the values of the two Uri attributes in the .appinstaller file to reflect the location to which the app is published.

A Release Pipeline Task That Modifies the Uris in the .appinstaller File

```powershell
- powershell: |
  [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
  $fileShare = "\\filesharestorageccount.file.core.windows.net\myfileshare\"
  $localFilePath =
    "$(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop\MsixDesktopApp.appinstaller"
  $doc = [System.Xml.Linq.XDocument]::Load("$localFilePath")
  $doc.Root.Attribute("Uri").Value = [string]::Format('{0}{1}', $fileShare,
    'MsixDesktopApp.appinstaller')
  $xName =
    [System.Xml.Linq.XName]"{http://schemas.microsoft.com/appx/appinstaller/2018}MainPackage"
  $doc.Root.Element($xName).Attribute("Uri").Value = [string]::Format('{0}{1}',
    $fileShare, 'MsixDesktopApp.appx')
  $doc.Save("$localFilePath")
displayName: 'Modify URIs in App Installer File'
```

In the task above, the URI is set to the UNC path of an Azure file share. Because this is where the OS will look for the MSIX package when you install and update the app, I’ve also added another command-line script to the release pipeline that first maps the file share in the cloud to the local Z:\ drive on the build agent before it uses the xcopy command to copy the .appinstaller and .msix files there:

```yml
- script: |
  net use Z: \\filesharestorageccount.file.core.windows.net\myfileshare
    /u:AZURE\filesharestorageccount
    3PTYC+ociHIwNgCnyg7zsWoKBxRmkEc4Aew4FMzbpUl/
    dydo/3HVnl71XPe0uWxQcLddEUuq0fN8Ltcpc0LYeg==
  xcopy $(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop Z:\ /Y
  displayName: 'Publish App Installer File and MSIX package'
```

If you host your own on-premises Azure DevOps Server, you may of course publish the files to your own internal network share.

If you choose to publish to a Web server, you can tell MSBuild to generate a versioned .appinstaller file and an HTML page that contains a download link and some information about the packaged app by supplying a few additional arguments in the YAML file:

```yml
- task: MSBuild@1
  inputs:
    solution: Msix/Msix.wapproj
    platform: $(buildPlatform)
    configuration: $(buildConfiguration)
    msbuildArguments: '/p:OutputPath=NonPackagedApp /p:UapAppxPackageBuildMode=SideLoadOnly  /p:AppxBundle=Never /p:GenerateAppInstallerFile=True
/p:AppInstallerUri=http://yourwebsite.com/packages/ /p:AppInstallerCheckForUpdateFrequency=OnApplicationRun /p:AppInstallerUpdateFrequency=1 /p:AppxPackageDir=$(Build.ArtifactStagingDirectory)/'
  displayName: 'Package the App'
  ```
  
The generated HTML file includes a hyperlink prefixed with the browser-agnostic ms-appinstaller protocol activation scheme:

```html
<a href="ms-appinstaller:?source=
  http://yourwebsite.com/packages/Msix_x86.appinstaller ">Install App</a>
```

If you set up a release pipeline that publishes the contents of the drop folder to your intranet or any other Web site, and the Web server supports byte-range requests and is configured properly, your end users can use this link to directly install the app without downloading the MSIX package first.
