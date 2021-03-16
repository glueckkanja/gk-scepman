# SCEPMan
>
> This repository was moved to [github.com/scepman/install](https://github.com/Scepman/install)
>
## Abstract
SCEPman implements an unattended Certificate Authority for Microsoft Intune based certificate deployment described in [this](https://docs.microsoft.com/en-us/intune/scep-libraries-apis) document:

“In Microsoft Intune, you can add third-party certificate authorities (CA), and have these CAs issue and validate certificates using the Simple Certificate Enrollment Protocol (SCEP). [Add third-party certification authority](https://docs.microsoft.com/en-us/intune/certificate-authority-add-scep-overview) provides an overview of this feature, and describes the Administrator tasks in Intune.”

The implementation is a .net core C# based Azure WebApp providing the [SCEP](https://tools.ietf.org/html/draft-gutmann-scep-13) and Intune API, using [Bouncy Castle](https://www.bouncycastle.org) to implement the necessary certificate request handling and [Azure Key Vault](https://docs.microsoft.com/en-us/rest/api/keyvault/) based RootCA and certificate signing. No other component needs to be involved, neither a database nor any other stateful storage except the Key Vault. That said, the concept will not need any backup procedures.
  
Please see https://glueckkanja.gitbook.io/scepman/ for full documentation.
  
## Deployment
For deployment please refer to the [current GitHub Repository](https://github.com/Scepman/install)