
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


The same DC can be assigned multiple FSMO roles, or even all five of them. The DC that is assigned a particular FSMO role is called the **role owner**.
But giving every role to single 
### Schema Master Role


THis is the role which is given to only of the DC. which means that in the forest only on Dc will have this Schema master role.
#### Purpose
- So this DC willl be responsible for making updates and chnages in the AD schema. which will be then replicated in the other DCs.
- Also here schema can be known as the blueprint of the AD, such that how the particular object should be created in the AD.
- For ex - while creating or adding a new object may be like user then what should be the schema or the attribute u have to consider while making it like first name , last name, addr, mobile no.(10 numeric digits limit) etc.
- Now if u want to make changes to the object like u need to add one more attribute to users that is - their employeeID then these type of things can be done only DC which has the Role OF SCHEMA MASTER.

### Domain Naming Controller

This role is also given to only one 