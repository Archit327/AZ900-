==NSG, ASG, Bastion and Virtual machine .

ASG- logical grouping of the virtual netwprk resources for easier maintainance
### AZURE VIRTUAL MACHINE

Virtual Machines are azure resources used for computing. Basically it gives you the flexibility of virtualization without having to buy and maintain the physical hardware that runs it.
Typically we choose a virtual machine when we need more control on the compute aspects.

#### How VMs are related to Virtual Network

VMs are integral part of networking infrastructure, and they interact with various networking components to communicate within a virtual network and with external resources.

#### Virtual network
VMs are deployed inside the virtual network in specific subnet.

#### Network Interface
each VM has its network interface and by that the VM gets connected to the Virtual Network.

#### IP addresses 
A VM after creation gets its PUBLIC and PRIVATE IPs, to send and receive traffic.

#### Network Security Groups
NSGs are often associated with VM to control inbound and outbound traffic

So, these basic uses in networking can justify how is VM related to the networking.

[ASG
![[Pasted image 20240108160558.png]]
]
#### What is NSG?

Azure Network Security Group(NSG) is a set of rule that control the inbound and outbound network traffic to network interfaces, VMs and ither resources within virtual network. 
NSGs contain security rules that filter network traffic by IP ADDRESS, port and protocol.    

#### What is the need for NSG?

Network Security Groups are crucial component for managing and controlling network traffic to and from azure resources.
They act a basic , stateful, rule based firewall that enables you to control access to your virtual machine and ither resources in an azure virtual network.

#### How to associate NSG?

we can associate NSG with VMs and SUBNETS
- VMs --> NSGs can be associated with VMs to control traffic to and from the VMs
- Subnets --> NSGs can be associated with Subnets to control traffic at the subnet level.

#### Filtering is based on
- Source/Destination IP addresses - define specific ip addresses and ranges
- Source /Destination Ports - control traffic based on specific ports.
- Protocols - TCP,UDP etc.



### Default Rules On NSGs


#### INBOUND RULES
![[Pasted image 20240108152159.png]]

By default NSGs deny all inbound and outbound traffic , we have explicitly define rules to allow traffic.

There are basically 3 default rules configured 
- allow vnet inbound
	- the action is allowed such that any vnet can recieve traffic
- Allow Azure loadbalancer Inbound
	- if source is azure load balancer and destination is any  then allow the traffic to reach any resource inside the subnet.
- Deny All Inbound


#### OUTBOUND RULES
![[Pasted image 20240108161019.png]]

There are basically 3 default rules configured 
- Allow Vnet Outbound
	- the action is allowed such that any two vnet can send traffic
-  Allow Internet Outbound
	- if source any and destination is INTERNET then allow the traffic to send out of the subnet/vm
- Deny All OutBound


### Creating a NSG rule

![[Pasted image 20240109200018.png]]
The NSGs are evaluated based on 5 Tuples ==Source, Source Port, Destination, Destination Port, Protocol==  
NSG rule are classified into two categories - Inbound Rule and Outbound Rule



#### LABS on NSGS basic rules established 
- RULE 1- RDP allowed for VM1 by MY IP adress
- Rule 2- RDP allowed for vm2 by MY IP address

![[Pasted image 20240109131458.png]]



AS a default since both the VMs are in the same Virtual Network Vm1 able to ping VM2 

![[Pasted image 20240109130838.png]]
But now added two rules ,
- 1.  deny vm1 to vm2 ping
- 2.  deny vm2 to ping vm1

![[Pasted image 20240108204544.png]]

vm1 cannot ping vm2

![[Pasted image 20240108204345.png]]

vm2 cannot ping vm1
![[Pasted image 20240108204456.png]]

