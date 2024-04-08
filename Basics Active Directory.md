

### Basic Active Directory Terms-->
- Domain
- Domain Controller
- Tree
- Forest
- Global Catalog
- DNS

#### Domain 

Basically it is a collection of objects , that can be user accounts , grps or computer accts. and more.

#### Domain controller 
Its a server with a active directory software installed. so this where the org manage the users , grps etc.

#### Tree

A collection of domains. For example if the company is a mnc and has offices in different locations, then each locations will have their own domains and which would be obviously managed by that particular branch locations.
Tree also shows the parent child connection behaviour such that the domain of the main branch of a company can be the PARENT DOMAIN and the second branch in other location having its own doamin will be connected to the parent domain and called as CHILD DOMAIN.

#### Forest


So in basic terms we can say FOREST is a collection of TREES.
what happens here as we see that single org can have multiple domains depending on their locations or may be multiple branches in same city. 
So what happens here is multiple domains make a TREE and combining multiple TREES make up the FOREST. 
Lets understand here with a diagram,

![[Pasted image 20240225003525.png]]

So here we can see there is a org named as abc and has 2 branches in 2 location i.e is US and India.
Also they have different domains as per there teams for example security teams and network team.
Both the parent domain is at the main branch location of the org i.e in US and then child domain consists of the data of India's branch.
Now here is the catch we can see that the org has two different trees having their own objects.
But all the child and parent domain are in same forest and this grouping is done and said to be a forest and this connectivity is called ACTIVE DIRECTORY TRUST . 
Now this term can be better explained by an example as shown in above figure the domain "us.securityc.abc" can access the folder that is located in "in.net.abc" . Now there is no direct connection if we see but since both the domain are in the same Forest therefore there is active directory trust.

Lastly Forest can be defined as **A collection of trees with shared resources available between all those domains that make up the forest.**

#### Global Catalog

It is nothing but a address book. 
All the trees consisting numerous domains and those domains containing data of all the objects is provided or available in the global catalog.
So if one of the domain in the forest wants to send the mail to someone in different domain then how will it do because that domain does not have the name of the person to whom he needs to send the mail.
So all the data of all the domains is settled down at Global Catalog, and sender can easily search the name of the user who is registered in different domain. 


#### DNS
So Domain Name System is a resolver. It converts the url into its IP Address.
So why do domains need the DNS it works globally as well such that if you www.google.com then dns will resolve its real IP address and make the site visible to you in your browser.
So the domain has its own DNS and its used as resolver for resolvoing the files or folder names to its address in that drive.


### Active Directory Tools
- Active directory user and computers (ADUC) -->
  this manages all the creation of users acc. and computer acc. or grps or Oorganizational Unit.
- Active Directory Administrative Center (ADAC) -->
  this works just same as that of aduc but it serves extra benefits and features which promote using it . Actually u can choose either of them to create accounts of user or grps or computers because both will reflect on the domain.l
- Active Directory module for Windows Powershell -->
  
- AD sites and services
- AD domain and trusts
- AD schema

### Group policy

Group policy also reduces misconfigurations.
I can make a change in a single location and I can have that change applied to several users, several computers, potentially thousands or tens of thousands of users and computers will process these settings.

Some of the benefits of using group policy would be the ability to apply consistent security settings. I can manage your Windows firewall, I can manage your password policy, we can manage your account lockout policy.

For example, if you were to enter the wrong password too many times, your account would become locked out.
I manage all those settings within group policy, so there's a whole host of security settings within
group policy.I can also manage desktop application settings.
We can deploy application software with some limitations.
Application software that we deploy has to be an MSI package.
Group policy does not support deploying exe files.
There are four locations that group policies can be linked to.
I can link a group policy object to the local computer that is the least common location.
We would apply group policy. If I apply a local group policy object, it only affects the local machine that you're actually using.

If you were to do that in a large environment, it would require physically going to each machine and making that change on each of your computers. If I wanted to view the local group policy, we could run the gpedit.msc command.
If your computer is not joined to the domain, local group policy would be the only policies you have.

For example, if I had an office in Miami and I also have an office in Seattle, I may only want domain
controllers between Miami and Seattle to replicate once every three hours. The way I control that is to create two separate sites. One site for Miami, one site for Seattle. Those sites are connected by a replication link.
That replication link lets me specify the schedule I want replication to occur on.
I could set a policy at that site level. If you were to do that, it would imply that I want you to get a certain setting because you are in the Seattle office.
If you physically leave the Seattle office and go to another location, we do not want you to get that policy.
In the past it was fairly common for default printers to be set at a site level.

The most common level that group policies are linked is the O level.
The O level is the last and lowest level of policy can be applied.
It's not possible to apply a policy directly to a user account or directly to a group or a computer
account. It can only be applied as low as the O.A group policy object that is linked to the organizational unit applies to all the users and computers in the organizational unit. The OO design is heavily influenced by the structure of group policy.


