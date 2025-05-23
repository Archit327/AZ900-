
reference - [(122) Azure Active Directory connect | Architecture - YouTube](https://www.youtube.com/watch?v=t1yHJ8DVO4Y)



![[Pasted image 20240223155200.png]]

Azure AD connect is a sof

Step 1:
![[Pasted image 20240223155345.png]]


Step 2:
![[Pasted image 20240223155356.png]]

Step 3:
![[Pasted image 20240223155420.png]]

Step 4:
![[Pasted image 20240223155429.png]]

## integrating azure ad with on-prem  with AZure AD connect Cloud Sync


### Prerequisites

1. Global administrator account on your Azure AD.
2. Tenant in Azure Active Directory.
3. On-Premise AD Server Administrator Access.

### Step by step guide to Follow

1. Create Azure AD and Create a Tanent  
    Tanent Type : Azure Active Directory
2. Configure the Organization Name and Domain Name. Review+Create
3. Download **Azure AD cloud sync** Agent and Move it to your Local On Premise Active Directory Server
4. Installing AADConnectProvisioningAgentSetup.exe on On Prem AD Server.
   
5. Connect/Authenticate Login to Azure AD using Global Admin Account created for Azure AD Directory.
   Here provide your credentials and get authenticated.
6. Configure Service Account - 
   ![[Pasted image 20240403092112.png]]
   
7. Connect On Prem Active Directory Domain
   Provide  domain Name and add your Directory.
8. Agent Installation and Configuration is complete. Provision on Azure AD Portal
9. Azure portal agent verification by > Azure AD> Azure AD Connect Cloud Sync > Review All Agents >It shows Active for my AD Machine.
10. Configure Azure AD Connect cloud sync Portal > Azure AD > Azure AD Connect > Manage Azure AD cloud sync > New Configuration.
    (It will automatically filled recently installed AD Connect Cloud Sync Agent Domain)
11. Testing with one task -- such that create 2 users in your Onprem  and see whether they get reflected on AZure AD.

IF YES THEN YOU MADE IT.


Entra ID and windows AD force sync powershell script

1. Import-Module ADSync
2. Start-ADSyncSyncCycle -PolicyType Initial