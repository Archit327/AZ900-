how to perform GRC 
and how to work in azure 
CCO dashboard
What is GRC?
Governance , Risk, Compliance

| Time Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 1: GRC introduction , Its Components and how it's utilized in an organization<br><br>Day 2: Governance , Azure Governance<br><br>Day 3: Blueprints, CCO dashboard<br><br>Day 4: Query , Presentation (Real customer requirement approach)<br><br>Day 5: Risk Assessment Introduction, Azure RA( focused on Gov), Process and documentation<br><br>Day 6: Query , Presentation<br><br>Day 7: Compliance introduction, Requirements<br><br>Day 8 : Regulations , laws and Act ;<br><br>Day 9: How to review a standard<br><br>Day 10: Query, Presentation |

## Day 1
### What is GRC
GRC stands for Governance - Risk - Compliance.
#### Governance

**Governance in cloud basically means governing that the RIGHT PEOPLE ARE DEPLOYING RIGHT RESOURCES WITH RIGHT CONFIGURATIONS. **

It  is a set of policies, rules, or frameworks that a company uses to achieve its business goals. It defines the responsibilities of administrator, such as the board of directors and senior management.
A good org would have governance as in
- **Ethics** - like proper punch in punch out
- **Accountability** - the higher authority should be answerable to any mishappening
- **Information sharing** - there should be transparency in some aspect
- **Policies** - set of rules which each and every employee should take care of

#### Risk
There can be any type of risk
- Financial risk
- physical risk
- Legal Risks
- Data breach risk
A good org will have proper risk management so that they can find security loopholes.
They will have time to time risk assessment procedures to have uptodate and have full command on the Environment.

#### Compliance
Compliance is the act of following rules, laws, and regulations. It applies to legal and regulatory requirements set by industrial bodies and also for internal corporate policies.
In GRC, compliance involves implementing procedures to ensure that business activities comply with the respective regulations. For example, healthcare organizations must comply with laws like HIPAA that protect patients' privacy.



![[Pasted image 20240829153411.png]]

## Governance in Azure

### Scope 
The level of hierarchy which will form azure environment.
![[Pasted image 20240829162347.png | 600]]

Their can be 6 level of Management grp as well

### RBAC
Role based Access Control
This maintains who can access what and what extent.
We have certain role for subscriptions basis.
- OWNER
- Contributor
- Reader
### Benefits of RBAC roles
- just restricts who can perform what 
- the role provided at high level is inherited to children scope
- roles can be applied at any scope
- custom role can be created 

### Resource Locks
 resource locks can be used for blocking at more granular level. 
 This can be applied to any resource and at any scope like subscription or resource grp. 
 and it will also be inherited from parent scope to child scope.
 There are two types of LOCK TYPE-
 - Read Only - the user can only read the resource but cannot make any changes and delete it.
 - delete - the user cannot delete the resource.


## Policy
Real time enforcement, compliance assessment and remediation at scale.
- we can do real time policy enforcements
- policy will help monitoring and alerting.
- it can be assigned to management grp , subcription and resource grps and those will be inherited to child scope.



DAY 2 - hierarchy, policies, blueprints, azure resource graphs, cost management

policy - basics, types