---
description: An overview of CI/CD solutions for MSIX app development
title: MSIX and CI/CD Pipeline Overview
ms.date: 01/11/2021
ms.topic: article
keywords: windows 10, uwp
ms.custom: RS5
---

# MSIX and CI/CD Pipeline Overview

You can use Azure Pipelines to create automated builds for your MSIX project in Azure DevOps by either using the Azure DevOps extension: *MSIX Packaging* Extension or by configuring your own yaml file. We’ll also show you how to perform these tasks by using the command line so that you can integrate with any other build system.

## Create a new Azure Pipeline

Begin by [signing up for Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up) if you haven't done so already.

Next, create a pipeline that you can use to build your source code. For a tutorial about building a pipeline to build a GitHub repository, see [Create your first pipeline](/azure/devops/pipelines/get-started-yaml). Azure Pipelines supports the repository types listed [in this article](/azure/devops/pipelines/repos).

To set up the actual build pipeline, you browse to the Azure DevOps portal at dev.azure.com/\<organization\> and create a new project. If you don’t have an account, you can create one for free. Once you’ve signed in and created a project, you can either push the source code to the Git repository that’s set up for you at https://\<organization\>@dev.azure.com/<organization\>/\<project\>/_git/\<project\>, or use any other provider, such as GitHub. You’ll get to choose the location of your repository when you create a new pipeline in the portal by clicking first on the **Pipelines** button and then on **New Pipeline**.


## Add your project certificate to the Secure files library

> [!NOTE]
>You should avoid submitting certificates to your repo if at all possible, and git ignores them by default. To manage the safe handling of sensitive files like certificates, Azure DevOps supports the [secure files](/azure/devops/pipelines/library/secure-files&preserve-view=true) feature.

To upload a certificate for your automated build:

1. In Azure Pipelines, expand **Pipelines** in the navigation pane and click **Library**.
2. Click the **Secure files** tab and then click **+ Secure file**.
3. Browse to the certificate file and click **OK**.
4. After you upload the certificate, select it to view its properties. Under **Pipeline permissions**, enable the **Authorize for use in all pipelines** toggle.
5. If the private key in the certificate has a password, we recommend that you store your password in [Azure Key Vault](/azure/key-vault/about-keys-secrets-and-certificates) and then link the password to a [variable group](/azure/devops/pipelines/library/variable-groups). You can use the variable to access the password from the pipeline. Note that a password is only supported for the private key; using a certificate file that is itself password-protected is not currently supported.

> [!NOTE]
> Starting in Visual Studio 2019, a temporary certificate is no longer generated in MSIX projects. To create or export certificates, use the PowerShell cmdlets described in [this article](../package/create-certificate-package-signing.md).

## Configure your pipeline
| Topic | Description |
|----------|-----------|------------|
| [MSIX Packaging Extension](msix-packaging-extension.md) | Leverage the Azure DevOps extension that will guide you through building and signing an MSIX package |
| [Configure CI/CD pipeline with YAML file](azure-dev-ops.md) | Configure your own yaml file |
