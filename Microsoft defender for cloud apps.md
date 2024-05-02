
Here ==Microsoft Defender for cloud Apps is considered  as CASB== that helps to identify and combat cyberthreats across the micorosft and other 3rd party cloud services.
Microsoft defender for cloud apps integrates with Microsoft solutions and providing simple deployment, centralized management, and innovative automation capabilities.
in simple words CASBs are the intermediaries between your users and all of the cloud services they access.

==microsoft defender for cloud apps== helps to apply monitor and security controls over the user and data. Just like a firewall it takes care what gets access and what not.

![[Pasted image 20240501141640.png]]

there are 4 elements in MD for cloud apps framework:
1. Discovery and control - 
   identify all the cloud apps and IAAS ans SAAS services used by your org, then how many are used by users. if you know the purpose the apps and services we can better manage the security and control the risk.
   
2. protecting the sensitive information anywhere in the cloud:
   now here the MD for cloud apps provide data loss prevention (DLP) capabilities that cover the various data leak points that exist in org.
   
3. protect against cyber threat and anomalies:
   detecting the unusaul behaviour accross any apps or services or user account. here MD for cloud apps gives multiple detection methods including user entity behavioural analytics (UEBA), and rule based activity detection whoch will show who is using the apps in your environment and how and when.
   
4. assess the compliance level of your cloud apps 
   always confirm that your apps are working with all the standards and regulatory compliances specific to your org .
   here MD for cloud apps helps to compare ypur apps and the usage against your compliance requirement, it orevents the data leak to non - compliant apps and limit the access to regulated data.


### Exploring the cloud apps with CLOUD DISCOVERY

- It is a unified place to see all the apps that can be used by your org and people, may be those apps can be sanctioned or not.
- It will display which apps are at high risk according to rank, which of them are non- compliant or against your regulatory compliance policies. Apps are given a risk score between 1 and 10.
- you can see the what types of apps are being used and alerts and risk identified in them
- You can see all top users of the app and from where do they come from.
- you can also customize what data you want to see in cloud discovery.

![[Pasted image 20240501145556.png  | 550]]
![[Pasted image 20240501145626.png | 450]]


 - View where discovered apps are located (based on their headquarters) in the **App Headquarters map**.
- If you find an app that poses a risk to your organization, you can flag it as **Unsanctioned** in the **Discovered apps** pane.

If your organization is using Microsoft Defender for Endpoint (or a similar solution), any unsanctioned app is automatically blocked.


### Conditional Access App control

Protect your data and apps with Conditional Access App Control.

Now Cloud discovery lets you know the overview of all the apps and users connectivity and how everything is being consumed.

But now we just dont need to see but also we need to stop breaches and leaks in real time before the org and its data comes to risk.
- Microsoft Defender for Cloud Apps integrates with identity providers (IdPs) to protect your data and devices with access and session controls through **Conditional Access App Control**.
- If you're using Microsoft Entra ID as your IdP, these controls are integrated directly into Defender for Cloud Apps.

- Conditional Access App Control lets you monitor and control user app access and sessions in real time.
- In microsoft entra id --> conditional access --> create policy
  u can create new policy allowing or restricting the users from access the particular cloud apps

![[Pasted image 20240501155603.png | 350]]

