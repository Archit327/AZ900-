
### Flexible Single Master operator(FSMO)

So FSMO are nothing but roles whoich are given to active directory DC.
Each DC can have only one role and then each DCs will perform the task according the role they get.

Although the name FSMO is changed to Operation MAster by Microsoft but still FSMO is in use.

## Purpose Of FSMO

History of FSMO roles-->
Earlier they used to give all the roles and specific operations to only one DC, so they used to have **single master model**.
But it can make conflicts such that if any time the primary DC is not available then whole working collapse.

Current Condition Of FSMO roles-->

So in order to overcome that problem microsoft changed the approach to **multi master model**.
Now they will have multiple DCs and each Dc will have specific role and operation.
What happens here is Now u can make changes to the AD through any of the Dc and then the update  will be replicated to each DC.

now here is one catch. So for ex in anytime if two DCs start making update then what will showw, so here is the approach of simple algo called as last writer winns. so accordingly the last update made will be reflected at the end. and the earlier changes will be discarded.

## What are 5 roles?

- Schema master
- Domain Naming Master
- Relative ID master (RID)
- Primary domain controller  (PDC)
- Infrastructure Master 

==IMPORTANT== 
There are two unique role and 3 for every domain.
Forest WIDE roles are unique and both the roles should not be given to a single DC. and 3 roles are DOMAIN WIDE and they can be divided between the DCs , those Dc can be the DC which have forest wide roles, also can be other DCs.
The same DC can be assigned multiple FSMO roles, or even all five of them. The DC that is assigned a particular FSMO role is called the **role owner**.
But giving all the roles to single DC is not recommended and not considered as best practise.
### Schema Master Role


THis is the role which is given to only one DC. which means that among the forest only one Dc will have this Schema master role.
#### Purpose
- So this DC willl be responsible for making updates and chnages in the AD schema. which will be then replicated in the other DCs.
- Also here schema can be known as the blueprint of the AD, such that how particular object should be created in the AD.
- For ex - while creating or adding a new object may be like user then what should be the schema or the attribute u have to consider while making it like first name , last name, addr, mobile no.(10 numeric digits limit) etc.
- Now if u want to make changes to the object like u need to add one more attribute to users that is - their employeeID then these type of things can be done only DC which has the Role OF SCHEMA MASTER.

### Domain Naming Controller

This is the role which is given to only one DC. which means that among the forest only one Dc will have this Domain naming role

#### Purpose
- It takes care that each Dc in the forest will have unique name.
- it is responsible for for adding and removing domains in the forest.
- Only this role has the power to add or remove the DC in the forest.
- It can also remove or add the cross references --> which means that the dc with this role DNM will be able to make or break the connection between other directories in the forest.
- So for ex if the company has two dc in different locations and want that these directories should not communicate with each other then DNM-DC  will do that, and if later want the connection to accesss the directories then too DC will again establish the connection.

### RID Relative ID Master role

So in a DOMAIN any one DC should have this role.

#### Purpose
- The purpose of the DC having this role is that it will be providing the RID pools to the other DCs in the domain when requested.
- So when a dc creates a new object or OU then they will be assigned with ==UNIQUE SECURITY IDENTIFIER==. 
- Now these Identifier have unique RID.
- SO each DC need a pool of RIDs with them to assign it to the new objects they create. And ther Pool of RIDs is provided by this RID MASTER.
- And when the pool of RID is finished for any DC then it will request the RID MASTER and get a new pool.
- RID master is also responsible for removing an object from one DC and putting it to another.

WHat happens with RID and SID?

So ==OBJECTSID== is created by using the domain SID(Security Identifier) and the RID pool value.
For ex--> We create a user "John" in DC whose SID is "A-001-123"
so while creating it the DC will have a certain RID pool from it will take a RID and give it to the user to be created so that it also has a unique value.
So now while creating it the OBject i.e the user JOHN will get its own unique ObjectSID which will be combination of DomainSID and RID

DomainSID+RID = ObjectSID
A-001-123 + 2001 = A-001-123-2001 now this will be the unique ObjectSID of that user which will be unique to the whole domain.

And this is how RID master DC works in the backend to provide the RID pools and see that every object has unique value.

==once the object is created there may be possibility that the name can be changed or other attributes can be changed but the ObjectSID will be unique always==

### PDC- Primary Domain Controller

### Infrastructure Master
Its a domain wide role can be given to any one DC inthe domain.

#### Purpose
- the purpose is quiet simple it translates the SIDs and Global Unique IDs of the objects.
- so when there are multiple Domains in the forest and u want to access some thing out side this domain i.e form another domain  then here Infra master will resolve the names of the object so that u cant see the original IDs of that object.
- Basically the security IDs of the objects are been secured because they should not leak out of the domain while connecting to other domain.

But now there is no use of IM role beacuse 
- now Microsoft Follows every Domain Controller as Global Catalog because of which the the integrity and consistency will be maintained.
- the forest are configured to use Recycle bin.


---

Some users' email messages containing images may be incorrectly flagged as malware and quarantined

Issue ID: EX873252
Affected services: Exchange Online
Status: Service degradation
Issue type: Incident
Start time: Aug 26, 2024, 7:39 PM GMT+5:30

User impact
Users' email messages containing images may be incorrectly flagged as malware and quarantined.

Scope of impact
Impact is specific to some users who are served through the affected infrastructure.

Root cause
A recent change to our Malware detection caused emails which contained a specific filetype signature to be incorrectly flagged as Malware.


Current status
Aug 26, 2024, 11:49 PM GMT+5:30
Our mitigation has successfully prevented new legitimate emails from mistakenly being flagged as malware. Emails sent after Monday, August 26, 2024 at 10:05 PM GMT+5:30
will not be impacted by this issue. We’re continuing to unblock and replay previously impacted emails, and many customers should already be experiencing relief from impact. Telemetry indicates that approximately 95 percent of that impacted emails have been resubmitted so far. 

Organizations will not need to action to resolve this issue, as the service will automatically replay the impacted emails. We currently estimate that all emails will be submitted within the next few hours, and we'll provide a more precise ETA once available.
Next update by:
Tuesday, August 27, 2024 at 1:00 AM GMT+5:30


History of updates
Aug 26, 2024, 11:29 PM GMT+5:30
Aug 26, 2024, 10:01 PM GMT+5:30
We've identified a recent change that may have affected our malware detection systems. We've implemented a mitigation intended to unblock legitimate emails that were mistakenly flagged as malware. We're working to replay the impacted emails and expect that affected emails will automatically be resent within the next several hours. We'll provide a more accurate ETA when it becomes available. In parallel, we’re continuing to investigate to determine if additional workstreams are needed to mitigate impact.
Aug 26, 2024, 8:58 PM GMT+5:30
We're analyzing provided Message IDs to isolate the reason that email containing images are being flagged as potential malware and determine the next steps to remediate the impact.
Aug 26, 2024, 7:40 PM GMT+5:30
We're reviewing service monitoring telemetry to isolate the root cause and develop a remediation plan.