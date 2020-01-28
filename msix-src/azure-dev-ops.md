#DevOps for MSIX Packaging and Deployment 

## Continuous Integration

If you want to set up CI for your MSIX packages, Azure Pipelines has great support. It supports Configuration as Code (CAC) through the use of YAML files and provides a cloud-hosted build agent that comes with all the software required to create MSIX packages pre-installed.

Before building the packaging project the same way the wizard in Visual Studio does using the MSBuild command line, the build process can version the MSIX package that’s being produced by editing the Version attribute of the Package element in the Package.appxmanifest file. In Azure Pipelines, this can be achieved by using an expression for setting a counter variable that gets incremented for every build, and a PowerShell script that uses the System.Xml.Linq.XDocument class in .NET to change the value of the attribute. Figure 2 shows an example YAML file that versions and creates an MSIX package based on a packaging project before it copies it to a staging directory on the build agent.

Figure 2 The YAML File That Defines the MSIX Build Pipeline

```
pool: 
  vmImage: vs2017-win2016
variables:
  buildPlatform: 'x86'
  buildConfiguration: 'release'
  major: 1
  minor: 0
  build: 0
  revision: $[counter('rev', 0)]
steps:
- powershell: |
   [Reflection.Assembly]::LoadWithPartialName("System.Xml.Linq")
   $path = "Msix/Package.appxmanifest"
   $doc = [System.Xml.Linq.XDocument]::Load($path)
   $xName =
     [System.Xml.Linq.XName]
       "{http://schemas.microsoft.com/appx/manifest/foundation/windows10}Identity"
   $doc.Root.Element($xName).Attribute("Version").Value =
     "$(major).$(minor).$(build).$(revision)";
   $doc.Save($path)
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

The name of the hosted virtual machine that runs Visual Studio 2017 on Windows Server 2016 is vs2017-win2016. It has the required UWP and .NET development workloads installed, including SignTool, which is used to sign the MSIX package after it has been created by MSBuild. Note that the PFX file shouldn’t be added to the source control. It’s also ignored by Git by default. Instead, it should be uploaded to Azure Pipelines as a secret file under the Library tab in the Web portal. Because it includes the private key of the certificate that represents the digital signature and identity of your company, you don’t want to distribute it to more people than necessary.

In large enterprises where you release your software to multiple environments in different stages, it’s considered a best practice to sign the packages as part of the release process and let the build pipeline produce unsigned packages. This not only lets you sign with different certificates for different environments, but it also gives you the ability to upload your packages to the Store where they’ll be signed by a Microsoft certificate.

Also note that secrets, such as the password for the PFX file, shouldn’t be included in the YAML file. Unlike variables that specify the targeted processor architecture and the package version, they’re defined and set in the Web interface.

You can add the YAML file to your Visual Studio Solution and check it in to your source code repository together with the rest of the code.

To set up the actual build pipeline, you browse to the Azure DevOps portal at dev.azure.com/<organization> and create a new project. If you don’t have an account, you can create one for free. Once you’ve signed in and created a project, you can either push the source code to the Git repository that’s set up for you at https://<organization>@dev.azure.com/\<organization>/<project>/_git/<project>, or use any other provider, such as GitHub. You’ll get to choose the location of your repository when you create a new pipeline in the portal by clicking first on the “Pipelines” button and then on “New Pipeline.”

On the Configure screen that comes next, you should select the “Existing Azure Pipelines YAML file” option and select the path to the checked-in YAML file in your repository.

The MSIX package that’s produced by the build can be downloaded and double-click installed on any Windows 10-compatible computer with the required certificate installed, or you could set up a CD pipeline that copies the package to a Web site or a file share where your end users can download it. I’ll come back to this in just a bit.

Auto-updates While an MSIX package is able to extract itself and automatically replace any older version of the packaged app that may be present on the machine when you install it, the MSIX format doesn’t provide any built-in support for automatically updating an app that has already been installed from an .msix file when you open it.

However, starting with the April 2018 Update of Windows 10, there’s support for an app installer file that you can deploy along with your package to enable automatic updates. It contains a MainPackage element whose Uri attribute refers to the original or an updated MSIX package. Figure 5 shows an example of a minimal .appinstaller file. Note that the Uri attribute of the root element specifies a URL or a UNC path to a file share where the OS will look for the updated files. When the URI differs between a currently installed version and a new app installer file, the deployment operation will redirect to the “old” URI.

Figure 5 An .appinstaller File That Will Look for Updated Files on \\server\foo

```
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

If you add the .appinstaller file in Figure 5 to the packaging project and set its Package Action property to Content and the Copy to Output Directory property to Copy if newer, you can then add another PowerShell task to the YAML file that updates the Version attributes of the root and MainPackage elements and saves the updated file to the staging directory:

```
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

If you create a release pipeline in the Azure Portal using the “Empty job” template and use the recently set up build pipeline as the source of the artifact to be deployed, as shown in Figure 6, you can then add the PowerShell task in Figure 7 to the release stage in order to dynamically change the values of the two Uri attributes in the .appinstaller file to reflect the location to which the app is published.

Figure 7 A Release Pipeline Task That Modifies the Uris in the .appinstaller File

```
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

In the task in Figure 7, the URI is set to the UNC path of an Azure file share. Because this is where the OS will look for the MSIX package when you install and update the app, I’ve also added another command-line script to the release pipeline that first maps the file share in the cloud to the local Z:\ drive on the build agent before it uses the xcopy command to copy the .appinstaller and .msix files there:

```
- script: |
  net use Z: \\filesharestorageccount.file.core.windows.net\myfileshare
    /u:AZURE\filesharestorageccount
    3PTYC+ociHIwNgCnyg7zsWoKBxRmkEc4Aew4FMzbpUl/
    dydo/3HVnl71XPe0uWxQcLddEUuq0fN8Ltcpc0LYeg==
  xcopy $(System.DefaultWorkingDirectory)\_MsixDesktopApp\drop Z:\ /Y
  displayName: 'Publish App Installer File and MSIX package'
```

If you host your own on-premises Azure DevOps Server, you may of course publish the files to your own internal network share.

Web Installs If you choose to publish to a Web server, you can tell MSBuild to generate a versioned .appinstaller file and an HTML page that contains a download link and some information about the packaged app by supplying a few additional arguments in the YAML file:

```
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

```
<a href="ms-appinstaller:?source=
  http://yourwebsite.com/packages/Msix_x86.appinstaller ">Install App</a>
```

If you set up a release pipeline that publishes the contents of the drop folder to your intranet or any other Web site, and the Web server supports byte-range requests and is configured properly, your end users can use this link to directly install the app without downloading the MSIX package first.


