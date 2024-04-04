
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
Forest WIDE roles are unique and should not be given both to a single DC. and 3 roles are DOMAIN WIDE and they can be divided between 
The same DC can be assigned multiple FSMO roles, or even all five of them. The DC that is assigned a particular FSMO role is called the **role owner**.
But giving all the roles to single DC is not recommended and not considered as best practise.
### Schema Master Role


THis is the role which is given to only one DC. which means that among the forest only one Dc will have this Schema master role.
#### Purpose
- So this DC willl be responsible for making updates and chnages in the AD schema. which will be then replicated in the other DCs.
- Also here schema can be known as the blueprint of the AD, such that how the particular object should be created in the AD.
- For ex - while creating or adding a new object may be like user then what should be the schema or the attribute u have to consider while making it like first name , last name, addr, mobile no.(10 numeric digits limit) etc.
- Now if u want to make changes to the object like u need to add one more attribute to users that is - their employeeID then these type of things can be done only DC which has the Role OF SCHEMA MASTER.

### Domain Naming Controller

This is the role which is given to only one DC. which means that among the forest only one Dc will have this Schema master role.

#### Purpose
- It takes care that each Dc in the forest will ahve unique name.
- it is responsible for for adding and removing domains in the forest.
- Only this role has the power to add or remove the DC in the forest.
- It can also remove or add the cross refernces --> whoch means that the dc with this role DNM will be able to make or break the connection between other directories in the forest.
- So for ex if the company has two dc in different locations and want that these directories should not communicate with each other then DNM-DC  will do that, and if later want the connection to accesss the directories then too DC will again establish the connection.

### RID Relative ID Master role

So in a DOMAIN any one DC should have this role.

#### Purpose
- The purpose of the DC having this role is that it will be providing the RID pools to the other DCs when requested.
- So when a dc creates a new object or OU then they will be assigned with ==UNIQUE SECURITY IDENTIFIER==. 
- Now these Identifier have unique RID.
- SO each DC need a pool of RIDs with them to assign it to the new objects they create. And ther Pool of RIDs is provided by this RID MASTER.
- And when the pool of RID is finished for any DC then it will request the RID MASTER and get a new pool.
- RID master is also responsible for removing an object from one DC and putting it to another.

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