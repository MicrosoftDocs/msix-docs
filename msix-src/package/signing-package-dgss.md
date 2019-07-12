---
Description: This article describes signing with DGSS
title: Sign an MSIX package with Device Guard signing
ms.date: 07/12/2019
ms.author: diahar
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---
# Signing an MSIX package with Device Guard signing
We are making it easier for you to sign your app. Device Guard signing (DGSS) is a Device Guard feature that is available in Microsoft Store for Business and Education. Signing allows enterprises to guarantee every app comes from a trusted source. Currently remote signing is convoluted and completely separated from all other processes of app packaging and repackaging. To fix this, our goal is to make signing repackaged MSIX apps easy.

## Before you get started 
Before getting started ensure that you go through these following documentation. The majority of this effort is to ensure that you are set up with the right permissions and configurations. 
1. Read background and overview of Device Guard signing go here https://docs.microsoft.com/en-us/microsoft-store/device-guard-signing-portal
2. To use this feature you will need a Microsoft Store for Business Account. To learn more go here. We will discuss more about the Device Guard signing roles below.  
https://docs.microsoft.com/en-us/microsoft-store/sign-up-microsoft-store-for-business
3. Ensure that you have access to the Microsoft store for Business API. To learn more go here https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app
4. Obtain your Azure Active Directory Token. To learn more go here https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-oauth-code

### Role(s) needed to use Device Guard signing 
In order to use the Device Guard signing, you will need to have the Device Guard signer role. This is the least privilage role that has the ability to sign. Other roles such as AAD Global Administrator, Store for Business Account Billing Owner can also sign. 

## Using DGSS with Signtool 
The first thing you will need is to obtain the AAD token in a json format. Ensure that you have the access token and a refresh token. We recommend obtaining the refresh token since your access token will expire in one hour. 

Once you have your AAD token in a json format, use the following command to call singtool to sign your package with DGSS

<Code>
signtool sign /fd sha256 /dlib DgssLib.dll /dmdf D:\temp19\token6b4023f8.json  test.msix
  
Note
1. Ensure that the package that you are signing publisher name matches the certificate you are signing with. Otherwise, signing will fail. You can find this out by going to Microsoft Store for Business and downloading your company's root certificate. 
2. We only support sha256

## Common Errors 
Here are a common list of errors that can occur
1. 0x800700d is a common error type that means that the AAD json file that was obtained format is invalid


