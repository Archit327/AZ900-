

It is a cloud based identity and access management service by microsoft azure.
It provides access to Office 365 services. like teams , exchange etc.
So when an user try to access the office srvices then the user will be authenticated  by the azure entra.

 So it can be used to create idenity of user , devices or create grps as well.
 
## Azure Active Directory Features 

- Azure active directory for developers - as a developer u can use AAD for using features like SSO and other authentication methods. for users to sign in to the apps or websites.
- Business to business (B2B) - with this feature u can interact to your user in very secure way who are  outside your org.
- B2C - with this feature u can allow the customers to login to your apps with their local or social accounts.
- Conditional Access - u can secure your apps and sign in process with applying conditional policy.
- Device Management - u can register or join your device with AAD or intune.
- Hybrid Identity- U can use AAD connect to provide single user identity for authentication and authorization to all resources.
- Privileged Identity Management - with this we can manage control and monitor the access within your org.
- Reports and monitoring - AAD provides reports which give insights of the resources and its security.

### AAD Licenses

![[Pasted image 20240229190508.png]]


### Security Defaults

- Require all users to register for multifactor authentication
- requiring administrator to do multifactor authentication
- requiring users to do multifactor authentication when necessary
- blocking the legacy authentication protocol - IMAP,SMTP etc multifactor authentication is mandatory for every user.
- MFA is required for accessing the following services
  - Azure portal
  - Microsft entra admin center
  - azure powershell
  - azureCLI
These policies apply to all the users accessing Azure Resource Manager services whether they are administrator or user.


AZure Ad Tenant -
It is like a instance of a azure active directory. 
An org can have multiple tenants , but its not recommended because each tenant is isolated from each other, so there is no benefit in creating 2 or more tenants for single org.

Azure Subscription -
for using or taking benefit of all the resources of the azure portal , the poerson should have subscription.
all the resources which are deployed through that subscription are being charged accordingly in  monthly basis.
a user can be provided with its own subscription as well as can be given some sort of permissions or role for accessing any other subscription.

Azure Active Directory Domain Services -

