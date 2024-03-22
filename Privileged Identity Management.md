
It is service in azure active directory that enables you to manages , control and monitor access to important resources in your org.
Now here the resources include all the resources in azure ad , azure and other microsoft services like M365 portal.

 
### Features provided by PIM
- Provide ***just in time*** privileged access to azureAD and azure resource 
- assign ***time bound*** access to resource using start and end dates
- require ***approval*** to activate privileged roles
- enforce ***multi factor authentication*** to activate any role
- use ***justification*** to understand why users activated
- get a notification when a privileged role is activated
- ***access review*** this feature will let the admin know who all have the privileged access and who all still need roles which means they can check the requests for roles.
- ***audit history*** - this will audit tell all the last 30 days  role activitation and access provided to the users.
  and also whose access is taken back or revoked.
  this audit history can be seen in Active directory security and activity reports.


### License requirement for PIM

In the basic 
- Azure AD Premium P2 license is required
- license includes P2
  - M365 E5

Remember -->
- the user u r trying to give PIM role should also have P2 license
- the person who is able to approve or reject activation requests in PIM should also have P2
- user who perform access review should also have P2
- user who set up and manage PIM, receive alerts and setup access review DONT REQUIRE P2


==IMPORTANT== --> Account which is used to enable PIM, becomes the member of Privileged Role Administrator and Security Administrator.


### What roles can be managed by PIM
- azure built-in role -
- azure AD built-in roles - 
- M365 roles - 

### Roles that cannot be managed using PIM
Rather PIM provides granular access control for Azure roles, but there are limitations regarding classic subscription administrator roles and specific Microsoft 365 role.
PIM does **not** allow management of the following ==classic subscription administrator roles== because in the early days of Azure, access to resources was managed using three classic subscription administrator roles
- ==**Account Administrator**:== Responsible for billing and subscription management.
- ==**Service Administrator**:== Manages resources using the Azure portal, Azure Resource Manager APIs, and classic deployment model APIs.
- ==**Co-Administrator**:== Also assists in managing resources within the subscription.


### Configure PIM-->

==Important
Eligible assigments --> these assignments are role which are managed by PIM, i.e the assignment will be given for particular period of time. These are also calle just in time role assignements.
Active assignements --> these assignments are role that are permanently assigned to given user==

First of all not everyone can create azure ad role for PIM only the admin or the Privileged role admin or the security admin can make changes in the role or assign role to the user. After getting the role user can either activate the role deactivate the role. and ofc if the role has to be approved then the user has ask for the approval.

Go to azure portal (through eligible account)>> search Privileged Identity Management in search bar >> azure ad roles >> settings >> select any role  from the given roles and edit them

![[Pasted image 20240321071016.png]]


Select any role here we take SECURITY READER
Now edit this role accordingly , here we get 3 sections to configure for creating the role for assignment 
- Activation
- Assignment
- Notification

### lets see one by one ACTIVATION -->
![[Pasted image 20240321071043.png]]
- Here determine the activation hr which will define that after what time the user needs to login again.
- then does the user need to do MFA or not for activation
- do you want the user to justify or not if yes then tick that box then user must justify while activation
- do want to get the info of the ticket after it gets activated such that the info will display all the details of the ticket
- approval should be done by the admin for getting the role activated
- down there mention who should approve the activation 

### Assignment 
![[Pasted image 20240321071206.png]]
- here we determine the two type of assignments 
  - Eligible (time-bound)– a role assignment where a user is eligible to request elevated admin roles, the user needs to request to activate the role.  
  - Permanent Eligible – a role assignment where a user is eligible permanently to request elevated admin roles, the user needs to request to activate the role.  
  - Active(time-bound) – a role assignment that does not require a user to request for role activation, when the role is assigned to a user it is automatically activated.  
  - Permanent Active – a role assignment like an Active assignment, but the role is assigned permanently to the user.
  
###Notification
![[Pasted image 20240321071324.png]]

- define the type of notification and to whom it should be send 