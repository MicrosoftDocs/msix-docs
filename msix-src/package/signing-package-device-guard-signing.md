---
description: This article describes how to sign an MSIX package with Device Guard signing, which enables enterprises to guarantee that apps come from a trusted source.
title: Sign an MSIX package with Device Guard signing
ms.date: 10/26/2020
ms.topic: article
keywords: windows 10, uwp, msix
---

# Sign an MSIX package with Device Guard signing

> [!IMPORTANT]
> Microsoft Store for Business and Microsoft Store for Education will be retired in the first quarter of 2023. You can continue to use the current capabilities of free apps until that time. For more information about this change, see [Evolving the Microsoft Store for Business and Education](https://aka.ms/windows/msfb_evolution)

> [!IMPORTANT]
> [Device Guard Signing Service v2](/microsoft-store/device-guard-signing-portal) (DGSS v2) is now available. 
> 
> May 2021 -
The existing web-based mechanism for the Device Guard Signing service v1 will be retired on June 9, 2021. Please transition to the PowerShell based version of the service (DGSS v2). A [NuGet package](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client) containing the required DGSS v2 components and migration documentation is available. Please read the Microsoft Terms of Use included in the NuGet package; note that the usage of DGSS implies acceptance of these terms. For any questions, please contact us at DGSSMigration@microsoft.com.

> [!NOTE]
> After downloading microsoft.acs/dgss.client.nupkg you can rename to .zip and extract contents for files and additional documentation and information

[Device Guard signing](/microsoft-store/device-guard-signing-portal) is a Device Guard feature that is available in the Microsoft Store for Business and Education. It enables enterprises to guarantee that every app comes from a trusted source. You can use SignTool in the Windows SDK and the DGSSv2 dlib in the NuGet package to sign your MSIX apps with Device Guard signing. This feature support enables you to easily incorporate Device Guard signing into the MSIX package building and signing workflow.

Device Guard signing requires permissions in the Microsoft Store for Business and uses Azure Active Directory (AD) authentication. To sign an MSIX package with Device Guard signing, follow these steps.

1. If you haven't done so already, [sign up for Microsoft Store for Business or Microsoft Store for Education](/microsoft-store/sign-up-microsoft-store-for-business).
    > [!NOTE]
    > You only need to use this portal to configure permissions for Device Guard signing.
2. In the Microsoft Store for Business (or or Microsoft Store for Education), assign yourself a role with permissions necessary to perform Device Guard signing.
3. Register your app in the [Azure portal](https://portal.azure.com/) with the proper settings so that you can use Azure AD authentication with the Microsoft Store for Business.
4. Get an Azure AD access token in JSON format.
5. Run SignTool to sign your MSIX package with Device Guard signing, and pass the Azure AD access token you obtained in the previous step.

The following sections describes these steps in more detail.

## Configure permissions for Device Guard signing 

To use Device Guard signing in the Microsoft Store for Business or Microsoft Store for Education, you need the **Device Guard signer** role. This is the least privilege role that has the ability to sign. Other roles such as **Global Administrator** and **Billing account owner** can also sign.

> [!NOTE]
> Device Guard Signer role is used when you are signing as an app. Global Administrator and Billing Account Owner is used when you sign as a logged in person.

To confirm or reassign roles:
1.	Sign in to the [Microsoft Store for Business](https://businessstore.microsoft.com/).
2.	Select **Manage** and then select **Permissions**.
3.	View **Roles**.

For more information, see [Roles and permissions in the Microsoft Store for Business and Education](/microsoft-store/roles-and-permissions-microsoft-store-for-business).

## Register your app in the Azure Portal

To register your app with the proper settings so that you can use Azure AD authentication with the Microsoft Store for Business:

1.	Navigate to https://portal.azure.com, and authenticate as a tenant Global Administrator
2.	Navigate to the **Azure Active Directory** Azure service.
3.	From the left side menu under **Manage** locate and select **App registrations**
4.	From the menu bar select **New registration**
5.	In the **Name** field enter **DGSSv2**.
    > [!NOTE]
    > The Name field is used for easy identification of the app registration in the Azure portal. Any name desired can be used. For the purpose of this demonstration, we use DGSSv2 simply to make it easy to identify.

6. Under **Supported account types** select the appropriate setting. 
    - **Accounts in this organization directory only (Single tenant)** – This option is recommended unless you have a specific need for a multitenant deployment. All user and guest accounts in your directory can use your application or API.
    - **Accounts in any organizational directory (Any Azure AD directory - Multitenant)** – This option is best for an organization that has multiple Azure AD tenants, but only needs a single point of trust for code signing. All users with a work or school account from Microsoft can use your application or API. This includes schools and businesses that use Office 365.
    - **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g., Skype, Xbox)** – This option is not recommended due to it being open to use by consumer level Microsoft accounts. All users with a work or school, or personal Microsoft account can use your application or API. It includes schools and businesses that use Office 365 as well as personal accounts that are used to sign into services like Xbox and Skype.
    - **Personal Microsoft accounts only** – Like the last option this option is also not recommended. This is not only because it allows personal accounts, but because this option only supports personal accounts.  Azure AD accounts are explicitly blocked. Personal accounts that are used to sign into services like Xbox and Skype

7.	In the **Redirect URI** drop down select **Public client/native (mobile & desktop)** from the drop-down selection menu. Enter https://dgss.microsoft.com in the text box.
8.	Click **Register**
9.	Toward the top right of the page locate the entry labeled Redirect URIs. Select the line below it labeled **0 web, 0 spa, 1 public client**
10.	Locate the entry labeled **Allow public client flows** in the Advanced settings section. Set this value to **Yes**
11.	Click **Save** at the top of the page
12.	From the left side menu select **API permissions**
13.	From the menu bar select **Add a permission.** In the fly out menu select the **APIs my organization** uses tab. In the search box enter **Windows Store for Business**

> [!NOTE]
> If Windows Store for Business does not show up in the list open a new browser tab and navigate to https://businessstore.microsoft.com then sign in as the tenant Global Administrator. Close the browser tab, then search again.

14.	Select **Windows Store for Business**, then select **Delegated permissions.** Check **user_impersonation**.
15.	Click **Add permissions** at the bottom of the page. From the left side menu select **Overview** to return to the DGSSv2 app registration overview.


## Get an Azure AD access token

Next, obtain an Azure AD access token for your Azure AD app in JSON format. You can do this using a variety of programming and scripting languages. For more information about this process, see [Authorize access to Azure Active Directory web applications using the OAuth 2.0 code grant flow](/azure/active-directory/develop/v1-protocols-oauth-code). We recommend that you retrieve a [refresh token](/azure/active-directory/develop/v1-protocols-oauth-code#refreshing-the-access-tokens) along with the access token, because your access token will expire in one hour.

> [!NOTE]
> If Windows Store for Business does not show up in the list open a new browser tab and navigate to https://businessstore.microsoft.com then sign in as the tenant Global Administrator. Close the browser tab, then search again.

The following PowerShell example demonstrates how to request an access token.

```powershell
function GetToken()
{

    $c = Get-Credential -Credential $user
    
    $Credentials = New-Object System.Management.Automation.PSCredential -ArgumentList $c.UserName, $c.password
    $user = $Credentials.UserName
    $password = $Credentials.GetNetworkCredential().Password
    
    $tokenCache = "outfile.json"

    #replace <application-id> and <client_secret-id> with the Application ID from your Azure AD application registration
    $Body = @{
      'grant_type' = 'password'
      'client_id'= '<application-id>'
      'client_secret' = '<client_secret>'
      'resource' = 'https://onestore.microsoft.com'
      'username' = $user
      'password' = $password
    }

    $webpage = Invoke-WebRequest 'https://login.microsoftonline.com/common/oauth2/token' -Method 'POST'  -Body $Body -UseBasicParsing
    $webpage.Content | Out-File $tokenCache -Encoding ascii
}
```

> [!NOTE]
> We recommend that you save your JSON file for later use.

## Obtain the Device Guard Signing version 2 DLL
To sign with Device Guard Signing version 2, obtain the **Microsoft.Acs.Dlib.dll** by downloading the [NuGet Package](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) that will be used to sign your package. This is also needed to obtain the root certificate. 

## Sign your package

After you have your Azure AD access token, you are ready to use SignTool to sign your package with Device Guard signing. For more information about using SignTool to sign packages, see [Sign an app package using SignTool](/windows/uwp/packaging/sign-app-package-using-signtool?context=%252fwindows%252fmsix%252frender#prerequisites).

The following command line example demonstrates how to sign a package with Device Guard signing version 2.

```cmd
signtool sign /fd sha256 /dlib Microsoft.Acs.Dlib.dll /dmdf <Azure AAD in .json format> /t <timestamp-service-url> <your .msix package>
```

> [!NOTE]
> * Certificates generated for Device Guard signing v2 are valid for one day. We recommend that you use one of the timestamp options when you sign your package. If you do not apply a [timestamp](signing-package-overview.md#timestamping), the signing will expire in one day and the app will need to be resigned.
> * Make sure that the publisher name in your package's manifest matches the certificate you are using to sign the package. With this feature, it will be your leaf certificate. For example, if leaf certificate is **CompanyName**, than the publisher name in the manifest must be **CN=CompanyName**. Otherwise, the signing operation will fail.
> * Only the SHA256 algorithm is supported.
> * When you sign your package with Device Guard signing, your package is not being sent over the Internet.

## Test

To test, download the root certificate by clicking [here](https://www.microsoft.com/pkiops/certs/microsoft%20enterprise%20identity%20verification%20root%20certificate%20authority%202020.crt) or by downloading the [NuGet Package](https://www.nuget.org/packages/Microsoft.Acs.Dgss.Client/) and obtaining it with the command:
```cmd
Get-RootCertificate
```

Install the root certificate to the **Trusted Root Certification Authorities** on your device. Install your newly signed app to verify that you have successfully signed your app with Device Guard signing. 

> [!IMPORTANT]
> In order to achieve isolation, deploy the WDAC CI policy to trust apps that are signed with DGSSv2. Be sure to read through the readme_cmdlets documentation and migration from DGSSv1 to DGSSv2 documentation that is included in the NuGet Package. 

## Common errors

Here are common errors you might encounter.

* 0x800700d: This common error means that the format of the Azure AD JSON file is invalid.
* You may need to accept the terms and conditions of Microsoft Store for Business before downloading the root certificate of Device Guard Signing. This can be done by acquiring a free app in the portal.
