---
Description: You can run scripts with the Package Support Framework to customize your desktop application for the user environment.
title: Run scripts with the Package Support Framework
ms.date: 09/20/2019
ms.topic: article
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
---

# Run scripts with the Package Support Framework

Scripts enable IT Pros to customize an application dynamically to the user's environment after it is packaged using MSIX. For example, you can use scripts to configure your database, set up a VPN, mount a shared drive, or perform a license check dynamically. Scripts provide a lot of flexibility. They may change registry keys or perform file modifications based on the machine or server configuration.

You can use the Package Support Framework (PSF) to run one PowerShell script before a packaged application executable runs and one PowerShell script after the application executable runs to clean up. Each application executable defined in the application manifest can have its own scripts. You can configure the script to run once only on the first app launch and without showing the PowerShell window so users won't end the script prematurely by mistake. There are other options to configure the way scripts can run, shown below.

## Prerequisites

To enable scripts to run, you need to set the PowerShell execution policy to `Unrestricted` or `RemoteSigned`. You can do this by running one of these commands:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

The execution policy needs to be set for both the 64-bit PowerShell executable and the 32-bit PowerShell executable. Make sure to open each version of PowerShell and run one of the commands shown above.

Here are the locations of each executable.

* 64-bit computer:
  * 64-bit executable: %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
  * 32-bit executable: %SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
* 32-bit computer:
  * 32-bit executable: %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe

For more information about PowerShell execution policies, see [this article](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6).

Make sure to include the following "StartingScriptWrapper.ps1" in your package, and locate it in the same folder where your executable is. You can copy it from the package support framework NuGet package (https://www.nuget.org/packages/Microsoft.PackageSupportFramework/).

## Enable scripts

To specify what scripts will run for each packaged application executable, you need to modify the [config.json file](package-support-framework.md#create-a-configuration-file). To tell PSF to run a script before the execution of the packaged application, add a configuration item called `startScript`. To tell PSF to run a script after the packaged application finishes add a configuration item called `endScript`.

### Script configuration items

The following are the configuration items available for the scripts. The ending script ignores the `waitForScriptToFinish` and `stopOnScriptError` configuration items.

| Key name                | Value type | Required? | Default  | Description
|-------------------------|------------|-----------|----------|---------|
| `scriptPath`              | string     | Yes       | N/A      | The path to the script including the name and extension. The path starts from the root directory of the application.
| `scriptArguments`         | string     | No        | empty    | Space delimited argument list. The format is the same for a PowerShell script call. This string gets appended to `scriptPath` to make a valid PowerShell.exe call.
| `runInVirtualEnvironment` | boolean    | No        | true     | Specifies whether the script should run in the same virtual environment that the packaged application runs in.
| `runOnce`                 | boolean    | No        | true     | Specifies whether the script should run once per user, per version.
| `showWindow`              | boolean    | No        | false    | Specifies whether the PowerShell window is shown.
| `stopOnScriptError`       | boolean    | No        | false    | Specifies whether to exit the application if the starting script fails.
| `waitForScriptToFinish`   | boolean    | No        | true     | Specifies whether the packaged application should wait for the starting script to finish before starting.
| `timeout`                 | DWORD      | No        | INFINITE | How long the script will be allowed to execute. When the time elapses, the script will be stopped.

> [!NOTE]
> Setting `stopOnScriptError: true` and `waitForScriptToFinish: false` for the sample application is not supported. If you set both of these configuration items, PSF will return the error ERROR_BAD_CONFIGURATION.


## Sample configuration

Here is a sample configuration using two different application executables.

```json
{
  "applications": [
    {
      "id": "Sample",
      "executable": "Sample.exe",
      "workingDirectory": "",
      "stopOnScriptError": false,
      "startScript":
      {
        "scriptPath": "RunMePlease.ps1",
        "scriptArguments": "\\\"First argument\\\" secondArgument",
        "runInVirtualEnvironment": true,
        "showWindow": true,
        "waitForScriptToFinish": false
      },
      "endScript":
      {
        "scriptPath": "RunMeAfter.ps1",
        "scriptArguments": "ThisIsMe.txt"
      }
    },
    {
      "id": "CPPSample",
      "executable": "CPPSample.exe",
      "workingDirectory": "",
      "startScript":
      {
        "scriptPath": "CPPStart.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runInVirtualEnvironment": true
      },
      "endScript":
      {
        "scriptPath": "CPPEnd.ps1",
        "scriptArguments": "ThisIsMe.txt",
        "runOnce": false
      }
    }
  ],
  "processes": [
    ...(taken out for brevity)
  ]
}
```
