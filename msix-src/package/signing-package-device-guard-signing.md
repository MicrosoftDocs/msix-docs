---
Description: This article describes signing with Device Guard signing
title: Sign an MSIX package with Device Guard signing
ms.date: 07/12/2019
keywords: windows 10, uwp, msix
ms.localizationpriority: medium
---

# Sign an MSIX package with Device Guard signing

Device Guard signing is a Device Guard feature that is available in the Microsoft Store for Business and Education. It enables enterprises to guarantee that every app comes from a trusted source. Our goal is to make signing repackaged MSIX apps a straightforward process.

## Prerequisites

Before getting started with Device Guard signing, read the following documentation and make sure you have the necessary permissions and configurations.

|Topic| Description |
|:---|:---|
|[Prerequisites for signing](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#prerequisites)| This section discusses the prerequisites for signing a Windows 10 app package. |
|[Using SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool?context=/windows/msix/render#using-signtool)| This section discusses how to use SignTool from the Windows 10 SDK to sign an app package.|
|[Device Guard signing](https://docs.microsoft.com/microsoft-store/device-guard-signing-portal)| This section provides an overview of the Device Guard Signing feature.|
|[Sign up for Microsoft Store for Business or Microsoft Store for Education](https://docs.microsoft.com/microsoft-store/sign-up-microsoft-store-for-business)| To use Device Guard signing, you need a Microsoft Store for Business Account. For more information about the permissions needed to perform Device Guard signing, see [this section](#roles-for-device-guard-signing). |
|[Quickstart: Register an application with the Microsoft identity platform](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)| Learn more about getting access to the Microsoft Store for Business API. |
|[Authorize access to Azure Active Directory web applications using the OAuth 2.0 code grant flow](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code)| Learn how to obtain your Azure Active Directory Token. |

### Roles for Device Guard signing

To use Device Guard signing in the Microsoft Store for Business and Education, you need the **Device Guard signer** role. This is the least privilege role that has the ability to sign. Other roles such as **Global Administrator** and **Billing account owner** can also sign.

To check the roles:

1. Go to Microsoft Store for Business Portal.
2. Click **Manage**.
3. Click **Permissions**.
4. View **Roles**.

For more information, see [Roles and permissions in the Microsoft Store for Business and Education](https://docs.microsoft.com/microsoft-store/roles-and-permissions-microsoft-store-for-business).

#### Accessing Device Guard signing with your app

To get Device Guard signing access with your app, your app will need to have the Device Guard signing role. The application will get its own AAD token. To do this, go to the [Azure portal](https://ms.portal.azure.com/) and when you register your app make sure it it has the Device Guard signing role assigned to it. 

## Using Device Guard signing with SignTool

Before using SignTool you must [obtain the AAD token in a JSON format](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Ensure that you have the access token and a refresh token. We recommend obtaining the refresh token because your access token will expire in one hour.

### Sample of Code to Obtain your AAD Token in JSON format

Here is a sample of a function that will obtain your AAD token. Note: depending on how you created your app in Azure AD, you may need to have a client secret. If you set your app as a native app, Public client (mobile & desktop) you do not need a client secret. 

```powershell
function GetToken()
{

    $c = Get-Credential -Credential $user

    $Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $c.UserName, $c.password
    $password = $Credentials.GetNetworkCredential().Password


    #replace <application-id> and <client_secret-id> with the Application ID and Client Secret ID from your Azure AD application registration
    $Body = @{
      'grant_type' = 'password'
      'client_id'= '<application-id>'
      'client_secret' = '<client_secret-id>'
      'resource' = 'https://onestore.microsoft.com'
      'username' = $user
      'password' = $password

    }

    $webpage = Invoke-WebRequest 'https://login.microsoftonline.com/common/oauth2/token' -Method 'POST'  -Body $Body -UseBasicParsing
    $webpage.Content | Out-File $tokenCache -Encoding ascii
}
```

After you have your AAD token, use the following command to call SignTool to sign your package with Device Guard signing.

`signtool sign /fd sha256 /dlib DgssLib.dll /dmdf D:\temp19\token6b4023f8.json  test.msix`
  
Make note of the following:

* You should ensure that the publisher name of the package you are signing matches the certificate you are using to sign the package. Otherwise, the signing operation will fail. To verify the publisher name, you can download your company's root certificate from the Microsoft Store for Business. 
* Only the SHA256 algorithm is supported.

## Common errors

Here are common errors you might encounter.

* 0x800700d: This common error means that the format of the AAD JSON file is invalid.
