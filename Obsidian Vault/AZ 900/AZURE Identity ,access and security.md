

#### Azure Active directory 
its a microsoft enterprise cloud based identity access management solution.
It can also sync with on-premises active directory and provide AUTHENTICATION to the other cloud based system.

Features of Azure AD
* Authentication -> it uses cloud based authentication protocol like OAuth2, SAML and WS-security for user authentication
* Network org -> each azure AD instance is called a tenant,under which user and grps come under.
* entitlement mangement  -> admins organise user into grps and then give grp access to apps and resource.
* devices -> with the help of mobile devices we can manage them
* desktop -> we can join Azure AD with MS intune
* servers -> it uses AZure AD services to mange server that live in the azure cloud virtual machine environment. 

#### AZURE AD LISCENCES
##### Azure Active Directory Free
user and grp mangement ,on-prem directory synchronisation , basic reports , self service password change for cloud users and single sign on.

##### Azure Active directory Premium P1

All features od Azure Ad free,as well as
access both on-prem and cloud resources ,dynamic grp manegment self service grp mangment Microsoft identity manager and password write back

##### Azure active directory Premium P2
all the features of free and P1 are accessible , as well as
conditional access and priviliged identity management


#### PAY AS YOU GO
this is like B2C feature

#### entra ID privides services such as
* **Authentication** --> it includes verifying of identity to access application and resources, self service password reset, multifactor authentication
* **Single sign on**--> enables to remember only one user at a timeand one passwrod such by once sign and then access multiple applications by that single sign in.
it helps such that if a user leaves the company then just all the access and managements are connected to that one user sign in then no need to check for every application or access procedures.

* **application management**--> we can manage both on-prem apps by using microsoft Entra ID .
* **device management**-->  along with account for individual people it also supports registration of devices . with the help of microsoft intune we can manage the devices , like for controlling device based access control or conditional access policy to restrict access attempts to only those allowed devices.


How to connect Onprem Active directory to Axure Active Directory?

one method to connect both the directory is using ==Microsoft Entra Connect== .
it synchronizes usser identities b/w on-prem and azure Entra ID .
for doing that we need two identity sets. and by using the features like SSO , Multifactor authentication, and self service password reset under both system.


![[Pasted image 20231127181314.png]]


difference between AD MFA vs SECURITY DEFAULT

| SECURITY DEFAULT                                                                | AD MFA                                                                             |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| it is free for Azure AD                                                         | need to have atelast azure AD premium P1 liscence                                  |
| it will be enabled at higher level i.e it will be allotted to the all the users | it can be done at smaller level as well i.e it can enabled even for the small grps |
| it will only use MFA application for authentication                             | we can use MFA , sms or call authentication options as well                        |
| easy to implement and dont require any sort of configuration at the admin level | Azure AD MFA is configured using conditional access                                                                                   |

 

#### Authentication methods

##### 1.SSO
=="Single sign on"== it enables the user to sign one time and enter the password one time to access multiple resources and application from different provider.

#### 2.Mutlifactor authentication
it works on getting an extra from of identification to ensure the sign process is valid and correct person is trying to enter.

it depends upon some three elemental categories 
1. something u know --> a challenging question
2. something u r --> it can be ur fingerprint or face scan
3. something u have --> itcan be otp or any code

##### 3.Posswordless authentication
some time  the user gets frustrated of remembering their passwords and usernames and as well as just to sign in go with multiple authentication.
so there is a passwordless authentication as well.
it just get signed in for one time or like get registered and enrolled then u every time u get to login again then u just need to provide something u r or u know that can be like finger print or may 4 digit pin.



#### FIDO2

This is the best security as it dont require any passwords or fingerprint or something u know as well .
it basiclly works on a devices for eg a pendrive the device has some number and as it gets connected for authentication just at that time it gets that authentication from the device(pendrive) that it its correct device and user gets logged in.
it can also be the bluetooth or NFC.

==here the security is increased as there is no pasword or code to show and remember which could be exposed.


### Azure External Identities

An external identity is a person or a device or service that is outside the org network.
==ENTRA external id handles all the ways how to securely interact with user outside ur org==

#### 1.B2B
collaborate with external user by letting them use their preferred identity to sign in to ur org's application.
establish a two way trust with another entra org for seamless collaboration.
connect with them through teams channels.
these b2b collaboration user r represented in ur directory as **GUEST**.

#### 2.B2C
publish ur application and other softwares and let the consumers and customers use them  , and theycan login to the application of their preferred ways.


==AZURE ROLE BASED ACCESS CONTROL==
==============================================

azure provides with this feature which enables us to provide the access to the user according to their role or task they need to do.

this helps us to give the least privilege to the user for the specific scope.

RBAC is applied on the user which can be the user or be the guest in ur entra
there are basically 4 types of roles user access admin, owner, contributor , reader.
the role can be scoped at 4 levels
starting from the resources, resources grp, subscription level and lastly management grp level.



## How is Azure RBAC enforced

whenever any action is initiated against an azure resource that passess through azure resource manager.
ARM manages the way to organise and secure your cloud resources.

Azure RBAC uses an allow model , its accordingly traced that at what level is the user gets its access granted .

Zero trust model
=====================

Zero trust simply defines that there is should be noone to be treated  as a safe zone, it works on a worst case scenarios and hence protect the data from with that expectation.

Zero trust model based on these guiding principles:
* **Verify explicitly**--> always authenticate and authorise based on all available data points.
* **sue least privilege access**--> limit the user with least required ccess for the job to be done, just-in-time and just-enough-access, risk based adaptiver policies and data protection.
* **assume breach**--> minimize the radius of breach and segment access. verify end-to-end encryption.

==the approach to accomplish zero trust model is by not trusting the inner user and devices inside the network, each user and device should be authenticated and then given the access rather than trusting the device and user is in the network and giving them access 


DEFENSE-IN-DEPTH
==========================


![[Pasted image 20231128152631.png]]


defense in depth is a set of layers which all secure the central data from attack.

every layer here tries to stop the attack but the main thing in this model is that there is not all the security to be covered only by one layer , every layer here stops the attack and warns the authority to take action.
it slows down the attack to directly reach to the central dataa.

Role of each layer-->
* ***physical layer***  protect the computer hardware in the data center
* ***identity and access layer*** controls access to infrastructure and change control 
* the ***perimeter*** layer uses distributed denial of services(DDoS) to protect from the attack when there is a large scale attack 
* the ***network*** layer limits the connection between the resources by segmentation and access control
* the ***compute*** layer secures the communication from the vms
* the ***application*** layer work on integrating the security in the application such that the application r secure by defaults
* the ***data*** layer is the main center which is mainly responsible for protecting the data from the attacks and save the data in protected in the storage          disks or in the database 



MICROSOFT DEFENDER FOR CLOUD
============================================


DEFENDER FOR CLOUD IS TOOL provided by microsoft for security posture mangement and threat protection. It monitors the cloud, on-premises, hybrid and multicloud environment to provide guidance and notifications aimed at strengthening ur security posture.

deploying defendr is easy as it is already integrated to azure.
this tool needs to secure the resources and track the security posture, cyber attacks and streamline security management .

