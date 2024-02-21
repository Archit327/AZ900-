
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


So defender fro business doesnt have the feature of deploying server to the endpoint thus it also brings one more license which can be added to the this particular defender for business license. that add-on license is nothing but ==defender for business server==.

- also defender for business license can be included in m365 business premium license.
- and if we talk about defender for endpoint 

![[Pasted image 20240221153437.png]]

So basically what can we conclude that defender for plan 1 and plan 2 are both included in m365 E3 and E5 licenses respectively such that plan 1 is enabled with m365 E3 and if u want plan 2 then u must have m365 E5 license. 
### Features of defender for business
![[Pasted image 20240221152318.png]]
### Defender for endpoint Plan 1 & 2

![[Pasted image 20240221152414.png]]


Both the license have almost same functionality of threat & vulnerability management , attack surface reduction , next gen protection , endpoint detection end response , auto investigation and remediation. 


But the usecase they are made or can be used are different such that defender for business is license used by small medium business organizations having employees upto 300.

But for defender for endpoint plan 1 and 2 are used by large organizations, which consist of employees more than 300.

But its not a compulsory for any small org to use the defender for business license it can also take the license of m365E5 its all upto them.


## ==comparision 2==

### defender for business server  and defender for server plan 1 & 2

requirements -

| defender for business server                                                   | defender fro server plan 1&2        |
| ------------------------------------------------------------------------------ | ----------------------------------- |
| requires either M365 business premium license or defender for business license | u have to enable defender for cloud |
| this can be done on Microsoft defender portal                                  | this can be done on azure portal    |
|                                                                                |                                     |
	