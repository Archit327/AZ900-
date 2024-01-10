AZure  virtual network is a service provided by the azure in order to make ur own private network.
It can made by using a virtual network instance , it serves as a instance of a srvice in which u can have resources to be deployed and work on them in secure manner with all the protection and monitoring .

As comparison to traditional network azure network is same like that maintaining ur own datacenter in ur on-prem , but it also offers some extra features like scalability , availability and isolation.


What can we do with azure vnet?
=

1. we can communicate to resources present in that network
   vm, storage accounts,firewalls,vpnGW,App service environment, AKS,VMSS,API mangement , web apps
   
2. connect two different virtual networks or on-prem
   by peering
   
3. connect to on-prem as well
    vpnGW-->1. P2S 2. S2S
   service end points
   express route
   
4. filtering the network traffic 
   NSG, ASG
   firewall
   nat GW
   
5. routing the network traffic
    route table
    bgp



Features of azure vnet
=
- its scope to single region
- peering and vpngw for cross vnet communication
- can have 1 more subnets with different and unique address spaces
- can use NSG and associate on the subnet and filter the traffic which will come to the subnet or the resource or which traffic will go out of the subnet
- can use ASG to grp the same related resources such that simlify the mangement of nsg rules.
  while writing the nsg rule i,e inbound or outbound rule u can place asg tags in the source and destination  

![[Pasted image 20231225215536.png]]

AZure portal can be used to deploy ur first vnet 
while creating  a vnet a default subnet is also created

there is a use something called as ip address which is required to be alloted while creating a vnet and the default subnet .


VIRTUAL NETWORK BASICS
=

- A vnet exist within a
  - specific subscription 
  - within specific region
  - it cannot span subscription nor region
- it contain 1 or more ip ranges
- it can have multiple subnets with a least cidr /29.    


while creating a vnet of any ip address by default a subnet is created with a cidr range equal or less than that of vnet.

so for example if a vnet is created with address space starting from 10.0.0.0/16 and a subnet is created within it such that with address space starting from 10.0.0.0/24.

so here for 
	vnet (10.0.0.0/16)
	 there will 16536 ip address
 for sunet (10.0.0.0/24)
	 there will be 256 ip address available


so for each subnet according to their CIDR range they get their owning ip count 
now in this case subnet gets 256 ips

here the irony is that in azure when creating a subnet 5 IPs are always taken by azure which will not be available for the user to assign, the rest can be used .

here 256-5 = 251 IPs will be available to use or assign any resource.



what are those 5 IPs used for
=


1. ***.0*** -->Network ip
2. ***.1*** -->Gateway
3. ***.2 & .3*** -->DNS
4. ***.255*** -->bradcast ip



