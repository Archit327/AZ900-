1. Manaagement grp
2. move the subscription form root management grp to new management grp
3. subscription and tenant id connenction
4. rg creation - based on prod , dev, test
5. vnets - based on prod, dev , test
6. subnets - app, web, serv
7. nsg - optional
8. asgs - optional
9. Firewall 
10. route table - optional
11. gateways
12. policies
13. RBACs



The incident detection description **"A user executed a valid accounts DCE/RPC command targeting a DC for the first time"** refers to a situation where a user has executed a Distributed Computing Environment / Remote Procedure Call (DCE/RPC) command aimed at a Domain Controller (DC), likely for the purpose of accessing or managing user accounts.

### **Breaking Down the Description:**

1. **DCE/RPC Command**:
   - **DCE/RPC** is a protocol that allows remote execution of commands over a network. It is commonly used in Windows environments for administration tasks such as managing user accounts, configuring services, or querying information from the DC.
   - This detection likely involves a command executed over this protocol to interact with the Domain Controller’s user account database.

2. **Targeting a DC (Domain Controller)**:
   - The **Domain Controller** is a crucial server in a Windows network that manages security authentication requests and user account information. Any changes or queries executed on the DC can have a significant impact on network security and access.

3. **First Time Execution**:
   - This suggests that the system is alerting you because this is the **first time** the user or this particular account has executed such a command targeting the DC, which may be unusual behavior depending on the user’s role or permissions.

### **Potential Risks:**
This detection can have several security implications:
- **Unauthorized Access**: If the user is not typically responsible for administering the DC, it may indicate that someone has gained unauthorized access to their account or is attempting to use their credentials inappropriately.
- **Privilege Escalation**: If the user normally doesn’t have elevated permissions, they might be attempting to escalate their privileges by interacting with the DC.
- **Reconnaissance or Lateral Movement**: Attackers often target Domain Controllers to gather intelligence about user accounts or to move laterally through the network, particularly after compromising a lower-privileged account.

### **How to Analyze This Detection:**

1. **Verify the User Account**:
   - Check the **user's role** in the organization. Are they an administrator, or do they have the necessary permissions to interact with the DC?
   - Investigate whether this user has a history of executing administrative commands or if this action is out of the ordinary for them.

2. **Examine the DCE/RPC Command**:
   - **Identify the command** that was executed. Was it an account query, a password reset attempt, or some other user management task? Commands like `NetUserAdd`, `NetGroupAdd`, or similar administrative actions could point to attempted privilege escalation.
   - Review any associated logs that may show the command in detail, such as **Windows Event Logs** (particularly for the domain controller).

3. **Review Access Logs**:
   - Check the **logs on the Domain Controller** for any additional suspicious activity around the time of the incident. Look for other commands or activities executed by the same user.
   - Review the **network activity** to see if there were any unauthorized login attempts, failed logins, or unusual traffic patterns (e.g., lateral movement attempts from one server to another).

4. **Cross-check User Activity**:
   - **Correlate the user’s activity** over time. Was this a one-time event, or has this user shown other signs of attempting to access sensitive areas of the network? 
   - Investigate whether the user’s machine has been compromised (e.g., via malware), which could explain the sudden change in behavior.

5. **Check for Known Exploits**:
   - Research whether any known vulnerabilities or exploits related to DCE/RPC could have been leveraged. Attackers often use such exploits to perform reconnaissance or elevate privileges within a network.

6. **Review Network Configuration and Security Policies**:
   - Ensure that **DCE/RPC traffic** is restricted to only authorized machines and users. Tighten any overly broad permissions or network access controls that might allow unauthorized users to interact with the Domain Controller.

### **Actions to Take**:

1. **Investigate and Contain**:
   - If this activity appears suspicious and unauthorized, **temporarily disable the user account** or **block access** to the Domain Controller while further investigation is carried out.
   - Isolate the user’s machine to prevent further unauthorized access or lateral movement within the network.

2. **Log Review and Forensics**:
   - Review detailed logs on the Domain Controller to look for any other unusual activity.
   - If there are signs of compromise, consider involving your incident response team for a deeper forensic analysis.

3. **Audit Permissions**:
   - Review the user’s permissions and roles in Active Directory. If they shouldn’t have access to the DC, consider reducing their privileges or removing any unnecessary administrative rights.

4. **Strengthen Authentication and Monitoring**:
   - Consider implementing **multi-factor authentication (MFA)**, particularly for administrative access to critical servers like Domain Controllers.
   - Use **SIEM tools** like Microsoft Sentinel to monitor and set up alerts for unusual activity around your Domain Controllers, such as new user logins or the execution of critical commands.

### **Summary**:
This detection indicates that a user has interacted with a Domain Controller using a DCE/RPC command for the first time, which could be normal administrative behavior or a sign of malicious activity, depending on the user’s role and history. To analyze it, investigate the user's actions, command details, and network activity, and take appropriate measures to contain any potential threats.



**TDMP** typically stands for **Technology Deployment and Management Plan** in the context of IT operations. When an IT operations team is "working on TDMP," it means they are involved in planning, deploying, and managing technology infrastructure or systems within the organization. 

### **Key Aspects of TDMP:**
1. **Technology Deployment**:
   - Planning and rolling out new hardware, software, network components, or cloud infrastructure.
   - Ensuring that the deployment aligns with the organization's strategic goals and IT architecture.
   
2. **Management**:
   - Monitoring and maintaining the deployed systems, ensuring they are secure, optimized, and running efficiently.
   - Managing changes, updates, and scaling of the technology stack as per operational needs.

3. **Project Phases**:
   - **Assessment**: Determining the current IT landscape and identifying the technology needs.
   - **Design and Planning**: Developing a detailed plan for deploying and managing the technology, including timelines, resources, and risk management.
   - **Deployment**: Executing the installation and configuration of systems.
   - **Monitoring and Support**: Ensuring the systems function as intended and resolving any issues.

If the IT Ops team is working on TDMP, they are likely focused on setting up, configuring, and maintaining critical IT systems or applications.