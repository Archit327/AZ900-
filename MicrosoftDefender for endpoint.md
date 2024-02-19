Naming conventions --
At ==launch time - it was named as WINDOWS DEFENDER ATP==
Then later changed to Microsoft Defender ATP
and ==currently it is Known as microsoft defender for endpoint==

Portal url - https://security.microsoft.com

What is MDE?
So basically to saying in very simple terms it can be said as a solution for  securing the endpoints from cyber threats , those endpoints can be the PCs , domain joined devices or other personal devices.

Why do we exactly use MDE?
The uses can be divided into 4 baseline stages-
1. Prevent - giving u the right setr of info to evaluate the environment and prevent the threat
2. Detect - it detects the threat by evaluating the whole environment where there is loophole or a certain resource which is not secure .
3. Investigate - It goes through all the possibilities and figures out the causes or the vulnerability in the environment or the resources 
4. Respond - after identifying all the vulnerability and causes of threat  it also helps to remediate them with most effective and convenient way possible.

---
### Components of MDE
![[Pasted image 20240218173711.png]]

While first time opening the wizard of MDE you have to provide the data center location. The locations present are US, UK, Europe.

---
### License Requirements
  
1. ==Defender for endpoint Plan 1 and Plan2==
2. ==Microsoft Defender for Business==
3. If u want to onboard servers ==MDE for servers==
4. ==Microsoft defender for BUSINESS server==

Here is a imp thing to note --> For every active license that u have assigned to a user u can get 5 devices for that user. 

----
### RBAC in MDE


---

Maximize available security capabilities and better protect your enterprise from cyber threats by deploying Microsoft Defender for Endpoint and onboarding your devices. Onboarding your devices will enable you to identify and stop threats quickly, prioritize risks, and evolve your defenses across operating systems and network devices.

This guide provides five steps to help deploy Defender for Endpoint as your multi-platform endpoint protection solution. It will help you choose the best deployment tool, onboard devices, and configure capabilities. Each step corresponds to a separate article.

The steps to deploy Defender for Endpoint are:

[![The deployment steps](https://learn.microsoft.com/en-us/microsoft-365/media/defender-endpoint/onboard-mde.png?view=o365-worldwide)](https://learn.microsoft.com/en-us/microsoft-365/media/defender-endpoint/onboard-mde.png?view=o365-worldwide#lightbox)

 [Step 1 - Set up Microsoft Defender for Endpoint deployment](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/production-deployment?view=o365-worldwide): This step focuses on getting your environment ready for deployment. 
   check liscence --> tenant configuration --> network configurations 
 [Step 2 - Assign roles and permissions](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/prepare-deployment?view=o365-worldwide): Identify and assign roles and permissions to view and manage Defender for Endpoint. 
   Microsoft recommends leveraging RBAC to ensure that only users that have a business justification can access Defender for Endpoint.
   giving least privileged access according to the role based access control
[Step 3 - Identify your architecture and choose your deployment method](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/deployment-strategy?view=o365-worldwide): Identify your architecture and the deployment method that best suits your organization.
![[Pasted image 20240213130154.png]]
## Step 1: Identify your architecture

Depending on your environment, some tools are better suited for certain architectures. Use the table below to decide which Defender for Endpoint architecture best suits your organization.

Expand table

| Architecture                        | Description                                                                                                                                                                                                                                                                                                                                                                |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cloud-native**                    | We recommend using Microsoft Intune to onboard, configure, and remediate endpoints from the cloud for enterprises that don't have an on-premises configuration management solution or are looking to reduce their on-premises infrastructure.                                                                                                                              |
| **Co-management**                   | For organizations that host both on-premises and cloud-based workloads we recommend using Microsoft's ConfigMgr and Intune for their management needs. These tools provide a comprehensive suite of cloud-powered management features, as well as unique co-management options to provision, deploy, manage, and secure endpoints and applications across an organization. |
| **On-premises**                     | For enterprises that want to take advantage of the cloud-based capabilities of Microsoft Defender for Endpoint while also maximizing their investments in Configuration Manager or Active Directory Domain Services, we recommend this architecture.                                                                                                                       |
| **Evaluation and local onboarding** | We recommend this architecture for SOCs (Security Operations Centers) that are looking to evaluate or run a Microsoft Defender for Endpoint pilot, but don't have existing management or deployment tools. This architecture can also be used to onboard devices in small environments without management infrastructure, such as a DMZ (Demilitarized Zone).              |
|                                     |                                                                                                                                                                                                                                                                                                                                                                            |
## Step 2: Select deployment method

Once you have determined the architecture of your environment and have created an inventory as outlined in the [requirements section](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/mde-planning-guide?view=o365-worldwide#requirements), use the table below to select the appropriate deployment tools for the endpoints in your environment. This will help you plan the deployment effectively.

Expand table

|Endpoint|Deployment tool|
|---|---|
|**Windows**|[Local script (up to 10 devices)](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-endpoints-script?view=o365-worldwide)  <br>[Group Policy](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-endpoints-gp?view=o365-worldwide)  <br>[Microsoft Intune/ Mobile Device Manager](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-endpoints-mdm?view=o365-worldwide)  <br>[Microsoft Configuration Manager](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-endpoints-sccm?view=o365-worldwide)  <br>[VDI scripts](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/configure-endpoints-vdi?view=o365-worldwide)|
|**Windows servers  <br>Linux servers**|[Integration with Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/azure-server-integration?view=o365-worldwide)|
|**macOS**|[Local script](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/mac-install-manually?view=o365-worldwide)  <br>[Microsoft Intune](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/mac-install-with-intune?view=o365-worldwide)  <br>[JAMF Pro](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/mac-install-with-jamf?view=o365-worldwide)  <br>[Mobile Device Management](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/mac-install-with-other-mdm?view=o365-worldwide)|
|**Linux servers**|[Local script](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-install-manually?view=o365-worldwide)  <br>[Puppet](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-install-with-puppet?view=o365-worldwide)  <br>[Ansible](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-install-with-ansible?view=o365-worldwide)  <br>[Chef](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-deploy-defender-for-endpoint-with-chef?view=o365-worldwide)  <br>[Saltstack](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-install-with-saltack?view=o365-worldwide)|
|**Android**|[Microsoft Intune](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/android-intune?view=o365-worldwide)|
|**iOS**|[Microsoft Intune](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/ios-install?view=o365-worldwide)  <br>[Mobile Application Manager](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/ios-install-unmanaged?view=o365-worldwide)|


 [Step 4 - Onboard devices](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/onboarding?view=o365-worldwide): Assess and onboard your devices to Defender for Endpoint.

 [Step 5 - Configure capabilities](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/onboard-configure?view=o365-worldwide): You're now ready to configure Defender for Endpoint security capabilities to protect your devices.


## Requirements

The following is a list of pre-requisites required to deploy Defender for Endpoint:

- You're a global admin
- You meet the [minimum requirements](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/minimum-requirements?view=o365-worldwide)
- You have a full inventory of your environment. The table below provides a starting point to gather information and ensure your environment is deeply understood by stakeholders, which will help identify potential dependencies and/or changes required in technologies or processes.

Expand table

|What|Description|
|---|---|
|Endpoint count|Total count of endpoints by operating system.|
|Server count|Total count of Servers by operating system version.|
|Management engine|Management engine name and version (for example, System Center Configuration Manager Current Branch 1803).|
|CDOC distribution|High level CDOC structure (for example, Tier 1 outsourced to Contoso, Tier 2 and Tier 3 in-house distributed across Europe and Asia).|
|Security information and event (SIEM)|SIEM technology in use.|


Microsoft Defender for Endpoint Plan 1 (P1)

P1 includes: - Next-generation protection (includes antimalware and antivirus) 
- Attack surface reduction 
- Manual response actions 
- Centralized management 
- Security reports 
- APIs 
- Support for Windows 10, Windows 11, iOS, Android OS, and macOS **devices**


Microsoft Defender for Endpoint Plan 2 (P2)

P2 includes Defender for Endpoint Plan 1 capabilities, plus: 
- Device discovery 
- Device inventory 
- Core Defender Vulnerability Management capabilities 
- Threat Analytics 
- Automated investigation and response 
- Advanced hunting 
- Endpoint detection and response 
- Endpoint Attack Notifications 
- Support for Windows client and server 
- Support for Non-Windows platforms (macOS, iOS, Android, and Linux)


