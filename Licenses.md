
![[Pasted image 20240220151909.png]]

![[Pasted image 20240220151933.png]]

----
Compare **Defender for Servers Plan 2** and **Defender for Business Servers**:

1. **Defender for Servers Plan 2**:
    
    - **Features**:
        - Includes all features from **Defender for Servers Plan 1**.
        - Provides **extended detection and response (XDR) capabilities**.
    - **Integration with Defender for Endpoint**:
        - Integrates seamlessly with **Defender for Endpoint**.
        - Offers features such as:
            - **Attack surface reduction** to lower the risk of attacks.
            - **Next-generation protection**, including real-time scanning, Microsoft Defender Antivirus, and more.
            - **Endpoint detection and response (EDR)** features like threat analytics, automated investigation and response, advanced hunting, and Endpoint Attack Notifications.
            - **Vulnerability assessment and mitigation** provided by Microsoft Defender Vulnerability Management (MDVM) as part of the Defender for Endpoint integration.
            - With **Plan 2**, you can also access premium MDVM features through the MDVM add-on.
    - **Licensing**:
        - **Charged per hour** instead of per seat, reducing costs by protecting virtual machines only when they’re in use.
    - **Automatic Provisioning**:
        - **Automatically provisions** the Defender for Endpoint sensor on every supported machine connected to Defender for Cloud.
    - **Unified View**:
        - [Alerts from Defender for Endpoint appear in the **Defender for Cloud portal**](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-select-plan)[1](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-select-plan).
2. **Defender for Business Servers**:
    
    - **Add-on** to **Defender for Business** and **Microsoft 365 Business Premium**.
    - [Provides a **single endpoint security experience** for both clients and servers within the **Microsoft Defender portal**](https://learn.microsoft.com/en-us/microsoft-365/security/defender-business/mdb-faq?view=o365-worldwide)[2](https://learn.microsoft.com/en-us/microsoft-365/security/defender-business/mdb-faq?view=o365-worldwide).

In summary, if you need comprehensive server protection with extended capabilities, **Defender for Servers Plan 2** is the way to go. [However, if you’re specifically looking for an add-on for business servers within the Microsoft Defender ecosystem, consider **Defender for Business Servers**](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-select-plan)[1](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers-select-plan)[2](https://learn.microsoft.com/en-us/microsoft-365/security/defender-business/mdb-faq?view=o365-worldwide).


---
Which type of org needs M365 Business Premium




---
## ==Comparision 1== 
### 1. Microsoft Defender for Business & MDE plan 1 & 2

It is an endpoint security solutions designed specially for small and medium size organizations or businesses(upto 300 employees).

![[Pasted image 20240220220041.png]]


pretty much all the features available in defender for endpoint plan 1 is same as of in defender for business.

requirements-->
license --> Defender for business comes with its own standalone license that would be shown down below
![[Pasted image 20240221151753.png]]


So defender fro business doesnt have the feature of deploying server to the endpoint it just provides the client's devices to onboard and secure that too 5 devices per client, thus it also brings one more license which can be added to the this particular defender for business license. that add-on license is nothing but ==defender for business server==.

- also defender for business license can be included in m365 business premium license.
- and if we talk about defender for endpoint we can see below

![[Pasted image 20240221153437.png]]

So basically what can we conclude that defender for endpoint plan 1 and plan 2 are included in m365 E3 and E5 licenses respectively such that plan 1 is enabled with m365 E3 and if u want plan 2 then u must have m365 E5 license. 
### Features of defender for business
![[Pasted image 20240221152318.png]]
### Defender for endpoint Plan 1 & 2

![[Pasted image 20240221152414.png]]


Both the license have almost same functionality of threat & vulnerability management , attack surface reduction , next gen protection , endpoint detection end response , auto investigation and remediation. 


But the usecase they are made or can be used are different such that defender for business is license used by small medium business organizations having employees upto 300.

But for defender for endpoint plan 1 and 2 are used by large organizations, which consist of employees more than 300.

But its not a compulsory for any small org to use the defender for business license it can also take the license of m365E5 its all upto them.

----
## What are the differences between Defender for Business and Defender for Endpoint Plans 1 and 2?

Both Defender for Business and Defender for Endpoint provide strong threat protection capabilities for your company's devices (computers, phones, and tablets, which are also referred to as endpoints). Defender for Business was designed for small and medium-sized businesses (up to 300 employees). With a simplified configuration process and device onboarding options, Defender for Business enables customers who don't necessarily have a security background to set up, configure, and use Defender for Business to protect company devices.

Defender for Endpoint is an enterprise endpoint security platform designed to help organizations like yours to prevent, detect, investigate, and respond to advanced threats. 

---

## ==comparision 2==

### defender for business server  and defender for server plan 1 & 2


#### Defender for business server
license needed--> 
	M365 business premium
	 defender for business
	after purchasing any of those license you can go to microsoft defender portal https://security.microsoft.com
- with this license user can access to defender portal to manage the endpoints 
- recommended to small and medium size business or org (upto 300 employees)
- In order to add on Microsoft Defender for Business servers, you'll need at least one paid license for [Defender for Business](https://learn.microsoft.com/en-us/microsoft-365/security/defender-business/mdb-overview?view=o365-worldwide) (standalone) or [Microsoft 365 Business Premium](https://learn.microsoft.com/en-us/microsoft-365/business-premium/m365bp-overview?view=o365-worldwide).
- There's a limit of 60 Microsoft Defender for Business servers licenses per subscription to Microsoft 365 Business Premium or Defender for Business.
- If preferred, you could use [Microsoft Defender for Servers Plan 1 or Plan 2](https://learn.microsoft.com/en-us/azure/defender-for-cloud/plan-defender-for-servers) instead to onboard your servers.
- You need one Microsoft Defender for Business servers license  for each instance of Windows Server or Linux, and you don't assign that license to users or devices.

Defender for server plan 1 & 2

license requirements -->
this defender for server plan 1&2 are subscription based plans so if u have m365 E3 or E5
also if u have M365 Business Premium 

so basically the licenses which provide access to services like azure active directory , which is essential for managing identities and access to azure resources.
so with azure ad integration we can use the particular license to sign in to azure portal , and that will get access to defender for sever plan 1& 2 according to the limitations of the license (for example with m365 e3 u get access to azure portal but u get  feature of server plan1 only)


#### Features of defender for business server and defender for server plan 1 & 2

while both solutions provide security for servers, Microsoft Defender for Business Server is geared towards smaller organizations with simpler needs, while Microsoft Defender for Server Plan 1 is designed for larger enterprises with more complex security requirements and a broader range of threats to mitigate. 
The choice between the two licenses would depend on factors such as the size and complexity of the organization's server infrastructure, as well as its specific security needs and budget considerations.