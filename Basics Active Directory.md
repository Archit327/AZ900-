

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