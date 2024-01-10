
[[AZURE VIRTUAL NETWORK]]


AZure virtual network and its subnets enable the azure resources to connect to each other and outside the network to the internet as well,or also with the on-premises client computer.

it work like an extension of ur on-premises network with resources that link other azure resources.

Azure VN provide the following key networking capabilites-->
* isolation and segmentation
* internet communication
* communication between azure and on-premises
* route the network traffic
* filter the network traffic
* it supports both public and private endpoints

public endpoint-->
it has a public ip and can be accessed from anywhere in the world

private endpoint -->
it has private ip enables to connect within the virtual network 


#### 1. isolation and segmentation
azure allows to create multiple isolated vnets and within that vnet we can manage multiple subnets with the defined or required address range.
while creating the vnets we assign a private ip or public ip address range a required and defined address range .
we can divide the ip address into subnets and allocate some part to each named subnet.

#### 2. internet communication
we can accesss the incoming connection from the internet by assigning a public ip address to an azure resources , or by putting the resource behind the public load balancer


#### 3. comm b/w azure resources

vnet can connect many azure service like vms,app service, aks, vmss etc
service endpoint help to connect azure resource type such as azure SQl dbs and storage acct ,
by this we can improve the security of the vnets within the resrces.

#### 4.comm b/w on-premises resrces
vnets allow us to connect the resrces together in on-premises as well as wuthin the azure subscription, that can be done in following ways:
* ##### point to site--> 
	computer from outside the network can be connected, in this case the client computer initiates an encrypted VPN connection to connect to Azure vn
* ##### site to site--> 
	it connect the on-premises VPN device  to azure VPN gateway in a VN. 
* #### azure express route-->
	it provides a dedicated private connectivity to azure that doesn't travel over the internet .
	express is suited for the environment where u need grater bandwidth and even high levek of security.

### 5. route network traffic 
- Route tables allow you to define rules about how traffic should be directed. You can create custom route tables that control how packets are routed between subnets.
- Border Gateway Protocol (BGP) works with Azure VPN gateways, Azure Route Server, or Azure ExpressRoute to propagate on-premises BGP routes to Azure virtual networks.

### 6.filter network traffic

Azure virtual networks enable you to filter traffic between subnets by using the following approaches:

- Network security groups are Azure resources that can contain multiple inbound and outbound security rules. You can define these rules to allow or block traffic, based on factors such as source and destination IP address, port, and protocol.
- Network virtual appliances are specialized VMs that can be compared to a hardened network appliance. A network virtual appliance carries out a particular network function, such as running a firewall or performing wide area network (WAN) optimization



### How data flows through Network 

#### Step1

* data moves through network based upon the three tables
  * MAC address table
    Mapping of **SWITCHPORT to MAC ADDRESS**
  * ARP Table/cache
    Mapping of **IP ADDRESS to MAC ADDRESS**
  * Routing table 
    Mapping of **IP NETWORK to INTERFACE or NEXT ROUTER**



#### AZURE VPN


Its a virtual private network used to connect outside machine into the azure network.
so if person being at home want to access the azure network privately by public ip then he can use the vpn through which he can get a private tunnel which is secure and protected from internet, and can easily access the azure network.

it is private and end to end encrypted

it follows the client server architecture
with help of the vpn gateway the client will be able to connect to the azure network or the sever side.

after connecting to the azure vpn we can conect to their private ip address.
for ex--> if a person is in remote location and wants to connect to the private network through vm then he can connect to the private network by connecting to azure vpn .


AZURE VPN :-
* Point to site (P2S)
  when there are very few amount of vms or may be clients that has be connected to the azure virtual network outside  the network then they can connect ti the network by P2S.
  they just hav configure vpn in the client machine and then they connect directly.
* Site to Site (S2S)
  when we need to connect to azure network to the on-premises network then S2S can be used to connect securely
  to connect to SITE to SITE we hav to cinfigure vpn devices.
  
*   express route
  it is total private network provide by MS
  the data doesnt go to private network its on their own made private network
  to setup express route we need to contact to the connectivity provider and it is costly as it is highly secure.  
  it lets u extend ur on-premises network into microsoft cloud over a private connection with the help of connectivity provider



![[Pasted image 20231117152316.png]]


#### VPN gateway
basically vpn gateway is specific type of vnet gateway that is used to send encrypted traffic between azure vnet and on-premises location over the public internet.

it transfering the info over a private secure tunnel from azure vnet to the on premises over a public internet.

vpn gateway instances r deployed in a dedicated subnet of the virtual network and enable the following


________

vpn gw will be on azure side
local net gw will be on on-premises vpn  devices that can be firewall or cloud 
activ standby in site to site is provided as
ghost instance 

dynamic routing


https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about#protocol




azure firewall
=
[[Network task 5 FIREWALL]]
3 types
baisc standard premium

IDS vs IDPS?

DNAT rules
network rules
application rules
NAT
=

source nat SNAT
dest nat DNAT
port nat PNAT


|      | srce | dest | port | translate S | trans d | trans p |
| ---- | ---- | ---- | ---- | ----------- | ------- | ------- |
| SNAT |   **   |      |      |    **         |         |         |
| DNAT |      |  **    |      |             |    **     |         |
| PNAT     |      |      |  **    |             |         |  **       |


