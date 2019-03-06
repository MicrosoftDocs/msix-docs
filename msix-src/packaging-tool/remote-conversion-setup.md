---
title: Remote conversion setup in MSIX Packaging Tool
description: Setup instructions for remote conversion
author: c-don
ms.author: cdon
ms.date: 02/26/2019
ms.topic: article
keywords: MSIX, MPT, MSIX Packaging Tool, remote IP
ms.localizationpriority: medium
ms.custom: 19H1
---

# Setup instructions for remote machine conversions 

> [!NOTE]
> Remote machine conversions is a new feature that is currently only available in the insider preview build. 
>
> You can join the [MSIX Packaging Tool Insider Preview Program](insider-program.md) to get access to this feature. 

In this preview release of the [MSIX Packaging Tool(1.2019.226.0)](insider-program.md#current-insider-preview-build), we enabled the ability to connect to a remote machine to run your conversion on. There are a few steps that you will need to take before getting started with remote conversions.  

PowerShell remoting must be enabled on the remote machine for secure access. You must also have an admin account for your remote machine.  If you would like to connect using an IP address, follow the instructions for connecting to a non-domain joined remote machine. 

## Connecting to a remote machine in a trusted domain 

To enable PowerShell remoting, run the following on the remote machine from an **admin** PowerShell window: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck 
```

### Port configuration 

If your remote machine is part of a security group(such as Azure), you must configure your network security rules to reach the MSIX Packaging Tool server.  

#### Azure 

1. In your Azure Portal, go to **Networking** > **Add inbound port** 
2. Click **Basic**
3. Service field should remain set to **Custom**
4. Set the port number to **1599** (MSIX Packaging Tool default port value – this can be changed in the Settings of the tool) and give the rule a name (e.g. AllowMPTServerInBound) 

#### Other infrastructure 

Make sure your server port configuration is aligned to the MSIX Packaging Tool port value(MSIX Packaging Tool default port value is 1599 – this can be changed in the Settings of the tool) 

## Connecting to a non-domain joined remote machine(includes IP addresses) 

For a non-domain joined machine, you must be set up with a certificate to connect over HTTPS. 

1. Enable PowerShell remoting and appropriate firewall rules by running the following on the remote machine in an **admin** PowerShell window: 

``` PowerShell
Enable-PSRemoting -Force -SkipNetworkProfileCheck  

New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled  True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP 
```
 
2. Generate a self-signed certificate, set WinRM HTTPS configuration, and export the certificate 

``` PowerShell
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My -KeyExportPolicy NonExportable).Thumbprint 

$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername"";CertificateThumbprint=""$thumbprint""}" 

cmd.exe /C $command 

Export-Certificate -Cert Cert:\LocalMachine\My\$thumbprint -FilePath <path_to_cer_file> 
```

3. On your local machine, copy the exported cert and install it under the Trusted Root store 

``` PowerShell
Import-Certificate -FilePath <path> -CertStoreLocation Cert:\LocalMachine\Root 
``` 

### Port configuration 

If your remote machine is part of a security group (such as Azure), you must configure your network security rules to reach the MSIX Packaging Tool server.  

#### Azure 

Follow the instructions to [add a custom port](#azure) for the MSIX Packaging Tool, as well as adding a network security rule for WinRM HTTPS 

1. In your Azure Portal, go to **Networking** > **Add inbound port** 
2. Click **Basic** 
3. Set Service field to **WinRM**

#### Other infrastructure 

Make sure your server port configuration is aligned to the MSIX Packaging Tool port value (MSIX Packaging Tool default port value is 1599 – this can be changed in the Settings of the tool) 
