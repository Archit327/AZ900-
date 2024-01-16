==NSG, ASG, Bastion and Virtual machine .

### ==Network Security Groups
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


### ==Creating a NSG rule

#### Properties that are displayed and to be determined for creating a **rule**

1. **NAME** -> unique name for the rule 
2. **Priority** -> It is  number between 100 to 4096.
		the rules are processed according the priority .
		lower is the number higher is the priority and vice versa.
3. **Source/Destination** -> These can be IP address , service tags , or Application Security 
				groups.
				NSGs are processed after the azure translates public IP to private IP for inbound
				and NSGs are processed before azure translates the private IP address to public IP address for outbound.
4. **Protocol** -> TCP , UDP, ICMP , which protocol we wanna use .
5. **Direction** -> The direction are two types INBOUND and OUTBOUND
6. **Port range** -> we can specify port range like 80 , 3389, 443 etc.


![[Pasted image 20240109200018.png]]
The NSGs are evaluated based on 5 Tuples ==Source, Source Port, Destination, Destination Port, Protocol==  
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


#### LIMITS for NSGs
==we can have upto 5000 NSGs per subscription
and 1000 NSG rules per NSG

---

### ==Application Security Groups

ASG- logical grouping of the virtual network resources for easier maintenance

![[Pasted image 20240108160558.png]]

ASG enables you to configure network security as a natural extension of an application's structure, allowing you to group virtual machine and define network security policies based on those groups


----
### ==AZURE VIRTUAL MACHINE

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

---

### ==Service Tags


---


Azure Bastion
=

Azure Bastion is a service in Microsoft Azure that provides secure and seamless Remote Desktop Protocol (RDP) and Secure Shell (SSH) connectivity to virtual machines (VMs) directly from the Azure portal, without the need for a public IP address on the VM. Bastion acts as a jump server, allowing you to securely connect to your VMs over Remote Desktop Protocol (RDP) or Secure Shell (SSH) using the Azure portal, thus reducing exposure to potential security risks.

Azure Bastion is a service in Microsoft Azure that provides secure and seamless Remote Desktop Protocol (RDP) and Secure Shell (SSH) connectivity to virtual machines (VMs) directly from the Azure portal, without the need for a public IP address on the VM. Bastion acts as a jump server, allowing you to securely connect to your VMs over Remote Desktop Protocol (RDP) or Secure Shell (SSH) using the Azure portal, thus reducing exposure to potential security risks.

### Key Features and Use Cases of Azure Bastion:

1. **Secure Remote Connectivity:**
   - **Encrypted Connections:** Bastion uses Remote Desktop Protocol (RDP) and Secure Shell (SSH) connections that are encrypted, ensuring secure communication between your local machine and the Azure VM.

2. **No Public IP on VMs:**
   - **Reduced Attack Surface:** VMs behind Azure Bastion do not need public IP addresses, reducing the attack surface and minimizing exposure to potential security threats.
3. **No hassle of managing Network Security Groups (NSGs):** 
	Azure Bastion is a fully managed platform PaaS service from Azure that is hardened internally to provide you secure RDP/SSH connectivity.
4. **Protection Against Port Scanning:** 
	   Your VMs are protected against port scanning by rogue and malicious users because you don't need to expose the VMs to the internet.
5. **Hardening in one place Only:** 
	   Azure Bastion sits at the perimeter of your virtual network, so you don’t need to worry about hardening each of the VMs in your virtual network.|
6. **Protection against Zero Day Exploits:** 
	   The Azure platform protects against zero-day exploits by keeping the Azure Bastion hardened and always up to date for you. 
   
 

### Use Cases:

1. **Remote Administration:**
   - Azure Bastion is ideal for securely connecting to VMs for administrative purposes, such as configuring settings, installing software, and performing maintenance tasks.

2. **Development and Debugging:**
   - Developers can use Bastion to access development and debugging environments hosted in Azure securely.

3. **Zero Trust Network Model:**
   - Bastion aligns with the zero-trust network model by minimizing direct exposure of VMs to the internet, providing secure access through a centralized and monitored service.

4. **Compliance Requirements:**
   - Bastion helps organizations meet compliance requirements by providing a secure and audited way to access VMs.

### How to Use Azure Bastion:

1. **Azure Portal:**
   - Access the Azure portal and navigate to the desired VM.
   - Click on the "Connect" button, and choose "Bastion" as the connection method.

2. **Authentication:**
   - Enter your Azure AD credentials, and if multi-factor authentication is enabled, complete the additional authentication step.

3. **RDP or SSH Connection:**
   - Once authenticated, you can establish an RDP or SSH connection to the VM directly from the Azure portal.

Azure Bastion simplifies and enhances the security of remote access to Azure VMs, providing a centralized and secure way to manage and administer your virtual machines in the cloud.




