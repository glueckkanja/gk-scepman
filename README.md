# SCEPMan
## Abstract
SCEPman implements an unattended Certificate Authority for Microsoft Intune based certificate deployment described in [this](https://docs.microsoft.com/en-us/intune/scep-libraries-apis) document:

“In Microsoft Intune, you can add third-party certificate authorities (CA), and have these CAs issue and validate certificates using the Simple Certificate Enrollment Protocol (SCEP). [Add third-party certification authority](https://docs.microsoft.com/en-us/intune/certificate-authority-add-scep-overview) provides an overview of this feature, and describes the Administrator tasks in Intune.”

The implementation is a .net core C# based Azure WebApp providing the [SCEP](https://tools.ietf.org/html/draft-gutmann-scep-13) and Intune API, using [Bouncy Castle](https://www.bouncycastle.org) to implement the necessary certificate request handling and [Azure Key Vault](https://docs.microsoft.com/en-us/rest/api/keyvault/) based RootCA and certificate signing. No other component needs to be involved, neither a database nor any other stateful storage except the Key Vault. That said, the concept will not need any backup procedures.
  
Please see https://glueckkanja.gitbook.io/scepman/ for full documentation.
  
## Deployment
### Register an application in Azure Active Directory

#### Add a new app registration in Azure Active Directory
1. Login to your [Azure Portal](http://portal.azure.com/) with an Admin Account.
2. Navigate to **Azure Active Directory**
3. Choose **App registrations**
4. Click **New registration**
5. Set supported account types to **Accounts in this organizational directory only**
![Screenshot](./docs/images/1.png)
6. Save the **Application (client) ID** somewhere because you will need it for the deployment
![Screenshot](./docs/images/7.png)

#### Create a client secret
1. Select the **Certificates & secrets** blade
2. Add a new client secret with **New client secret**
3. Define a Description and set expiration to **Never**
4. Save the generated secret somewhere because you are not able to look it up again

#### Set API permissions
1. Select the **API permissions** blade
2. Click **Add a permission** to grant required permissions
![Screenshot](./docs/images/4.png)
3. Select **Intune** 
4. Choose **Application permissions** as the permission type
5. Click **scep_challenge_provider** and confirm with **Add permission**
6. Click **Add a permission** once again
7. Select **Microsoft Graph**
8. Choose **Application permissions** as the permission type again
9. Expand **Directory** and check **Directory.Read.All** and confirm with **Add permission**
10. Click **Grant admin consent** and confirm the displayed dialog with **Yes**
![Screenshot](./docs/images/5.png)

Your API permissions should be configured like this:
![Screenshot](./docs/images/6.png)

### Deploy to Azure

When the app registration is done use this button to deploy SCEPMan to your Azure subscription.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fglueckkanja%2Fgk-scepman%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Instead, you can also <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fglueckkanja%2Fgk-scepman%2Fmaster%2Fazuredeploy-beta.json" target="_blank">Deploy the Beta Channel</a>.

When clicking the deploy button you will see this form dialog
![Screenshot](./docs/images/8.png)
1. Select an existing resource group or create a new one. The SCEPMan resources will be deployed in this resource group.
2. Set the location according to your location
3. Insert the GUID of the app registriation which you have created in the steps before
4. Insert the client secret of this app registration
5. Define a name for key vault, app service plan and web site
6. Agree to the terms and conditions by clicking the checkbox
7. Click **Purchase**

Sometimes it is necessary to restart the app service before SCEPMan runs properly.

### Create root certificate

* Send a post request to certsrv/mscep/mscep.dll/pkiclient.exe/create-root
* Please look into the logs in case of any error.
