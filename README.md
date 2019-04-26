# SCEPMan
## Abstract
SCEPman shall implement a fully unattended Certificate Authority for Microsoft Intune based device certificate deployment described in [this](https://docs.microsoft.com/en-us/intune/scep-libraries-apis) document:

“In Microsoft Intune, you can add third-party certificate authorities (CA), and have these CAs issue and validate certificates using the Simple Certificate Enrollment Protocol (SCEP). [Add third-party certification authority](https://docs.microsoft.com/en-us/intune/certificate-authority-add-scep-overview) provides an overview of this feature, and describes the Administrator tasks in Intune.”

The intended implementation is a .net core C# based Azure WebApp providing the [SCEP](https://tools.ietf.org/html/draft-gutmann-scep-13) and Intune API, using [Bouncy Castle](https://www.bouncycastle.org) to implement the necessary certificate request handling and [Azure Key Vault](https://docs.microsoft.com/en-us/rest/api/keyvault/) based RootCA and certificate signing. No other component should be involved, neither a database nor any other stateful storage except the Key Vault. That said, the concept will not need any backup procedures.

## Deployment
### Register an application in Azure Active directory

* Go to Azure active directory
* Click on App registrations
* Click new application registration
![](https://user-images.githubusercontent.com/24998910/52491764-d7d7d300-2bed-11e9-8557-d81f6268eb49.jpg)
* Enter scepman-app as name
* Select Web app/API in application type
* In sign-on URL type the URL of the SCEPMan Website deployed in your tenant.  
* Click create

  ![](https://user-images.githubusercontent.com/24998910/52492098-8a0f9a80-2bee-11e9-85d0-056c234a9acc.jpg)
* Note down **application id** 

  ![](https://user-images.githubusercontent.com/24998910/52492194-b9bea280-2bee-11e9-919a-beea90198cdd.jpg)
* Generate a **key**. Select Keys

  ![image](https://user-images.githubusercontent.com/24998910/53296751-ddadf500-3838-11e9-9c0c-9c3a4190d920.png)

* Provide a description of the key, and a duration for the key. When done, select Save.
* After saving the key, the value of the key is displayed. Copy this value because you aren't able to retrieve the key later. You provide the key value with the application ID to sign in as the application. Store the key value where your application can retrieve it.

  ![](https://user-images.githubusercontent.com/24998910/52493652-09eb3400-2bf2-11e9-95f4-f02e9528c3da.jpg)

* Set API permissions
![](https://user-images.githubusercontent.com/1116271/53331414-1e694500-38f1-11e9-90de-ab4137135919.png)

### Deploy to Azure

When the app registration is done use this button to deploy SCEPMan to your Azure subscription.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fglueckkanja%2Fgk-scepman%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Instead, you can also <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fglueckkanja%2Fgk-scepman%2Fmaster%2Fazuredeploy-beta.json" target="_blank">Deploy the Beta Channel</a>.


### Create root certificate

* Send a post request to certsrv/mscep/mscep.dll/pkiclient.exe/create-root
* Please look into the logs in case of any error.