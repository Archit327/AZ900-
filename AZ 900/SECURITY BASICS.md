==%% 
5 pillars 
identify
protect 
detect
respond
recover

diff feature in different liscence %%
#### NEED of SECURITY
- ease of visibility - the data is on the internet and and many can seee the data  , since many cloud services are accessed outside the corporate network.
- access management - who can access the data or information or trhe resources on the cloud role based access should be used to give least privileged access  
- compliance - the security and other resources are working properly or not are they compliant or not.
- misconfigurations - is everything up to date or not , the passwords and keys are at safe place 


### AZURE SECURITY

Microsoft Entra ID 
	It enables you to sign in to access the microsoft cloud and its applications
	It provides and IAM service which is managed by the org
	it provides services such as - Authentication, Single sign on, Application management, device management

Microsoft Entra Domain Services
	It provides managed domain services like domain join, grp policy , lightweight directory access protocol
	when u create a MEDS managed domain , u define a domain name, which is unique. After which two windows server domain controller are deployed in selected azure region.
	These DCs are handled and managed by azure platform, including backups and encryptions.


#### Azure Authentication Methods

![[Pasted image 20240110134338.png]]


Azure uses multiple authentication methods to verify the person or an identity who they are. 
It just verifies that "you are who you say u r" .


- Single sign onn
	It enables the user to sign in once and use that credentials to access multiple resources and application.
	it is used to avoid remembering more IDs and more passwords
	In SSO u need to remember only 1 ID and 1password

- Multifactor Authentication
	It take more than authentications mode to verify.
	It can be OTP, ur finger print, or may be face, it just verifies that is the authorised person is accessing or not.
	It provides additional security for identities by requiring two or more elements to fully authenticate.
	- something u know - DOB
	- something u have - OTP /PIN
	- something u r - fingerprint, face scan

- passwordless authentication
	u can use ur device enrollment , like once azure knows that u use this particular device then once u provide with the pin or finger print , it authenticates without using a password.
	Like windows Hello for business
	microsoft authenticator App in ur mobile
	FIDO2 security- these can be USB devices, bluetooth or NFC, a hardware device handeling the security makes it more secure.
	

#### Conditional Access
This tool is used by Microsoft Entra ID to allow access to resources based on identity signals. These signals include who the user is, where the user is (its location) and and what device the user is using to access.

it can be helpful to 
	block the access from unknown locations
	block the access from unknown device and network or signals


#### Role based access control

![[Pasted image 20240110140601.png]]


different role like ==Reader , Contributer , Owner== 
according to the identities authorization and privileg.
it works on a concept of least privileged access

Azure RBAC doesn't enforce access permissions at application or data level , it is already scoped at portal CLI or powershell level.
Applicaton and data security must be handled by applications.

#### Zero Trust Model

It works on a worst case scenario and protects the resources with such expectation.

Zero trust model relies on 3 Principles;
- verify explicitly - always authenticate and authorize on the given data points
- use least privileged access - limit user access with Just-in-time and just-enough-access, risk based adaptive policies and data protection.
- assume breach - minimize segment access , verify end-to-end encryption drive threat detection, and improve defense.


#### Defense In Depth

The approach of defense in depth is to secure the data or information from being stolen by those who are not authorized to access it.

![[Pasted image 20240110152506.png]]      


Each layer provides protection so that if 1 layer is breached , the subsequent layer will be already in place to protect from further exposure.
these layer slow down the attack and notifies the security team come-up and act upon it.

- The physical security layer is the first line of defense to protect computing hardware in the datacenter.
- The identity and access layer controls access to infrastructure and change control.
- The perimeter layer uses distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for users.
- The network layer limits communication between resources through segmentation and access controls.
- The compute layer secures access to virtual machines.
- The application layer helps ensure that applications are secure and free of security vulnerabilities.
- The data layer controls access to business and customer data that you need to protect.


### ==Microsoft Defender For Cloud  

It is security tool by microsoft azure for monitoring security posture management and threat protection. It monitors ur cloud , on-premises, hybrid and multicloud environment to provide guidance and notification aimed at strengthening ur security posturet.


==It provide tools to harden the resources , track security posture , protection against cyber attacks and streamline management. 

Not only for azure cloud and its resources but also for hybrid and multicloud microsoft defender have plans for azure arc cloud security posture management.(CSPM)


![[Pasted image 20240110204356.png]]