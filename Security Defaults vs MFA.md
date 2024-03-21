


# Security Defaults 

This security settings are preconfigured by microsoft.
When u create  a new tenant u will get security defaults enabled. 
which can be further disabled.
to disable go to azure portal; --> entra ID --> Properties --> Manage Security defaults --> Select ==NO== 



What does Security Defaults do in terms of security -->
- requires all the user to register for MFA
  its like u cant select the particular number of users to register for MFA. everyone has to register
- requires the user and administrator to do MFA whenever required.
- It blocks the legacy authentication protocols - like many org will be using legacy email protocols like POP3 but security of the org is compromised by using those legacy protocols thus Security defaults block them.
- Protect privileged activities like access to the Azure portal - org can use many azure services like entra admin center , azure powershell, CLI etc so these are managed by ARM api thus they work with high privileged actions thus it requires the user to provide more info about themselves before getting the access to the particular portal or services.

## Limitations of security defaults->

- SD can be enabled for every user in the tenant , you can give it to particular set of user or grp.
- You can only use authenticator app for authentication method



| Security defaults                                        | Azure ad MFA                                                                                                   |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Its free feature and is enabled from begining.           | U need a atleast Azure Active Directory Premium P1 license . Then the license which include P1 are also valid. |
| It can be enabled for the user in the tenant.            | MFA can be enabled for set user or grps                                                                        |
| Only authenticator app is used for authentication method | Multiple authentication methods are available like SMS, Call, Authenticator App                                |
| Its easy to implement                                    | It can be configured using Conditional access policies                                                         |


To configure AZure AD MFA -->
go to Entra ID--> security --> conditional policy --> new policy --> give a name for the policy --.
### now for USER or Workload Identities - select accordingly 
- None 
- All users
- Select User and grps 

u can exclude some users from that policy as well

### now for Cloud apps and actions
here u have to configure the apps or add the apps on which u want to apply this policy
and the click on it and again select accordingly
- None
- all cloud apps 
- select apps

Now go to GRANT blade and there select the type of control access enforcement 
for eg --> require MFA 


At the end make sure ==enable Policy is Turned 
"On"==  then click create.


