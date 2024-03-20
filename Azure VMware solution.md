

- VMware, Inc. is an American cloud computing and virtualization technology company headquartered in California. 
- VMware was the first commercially successful company to virtualize the x86 architecture. (Wikipedia) 
- VMWare is the leader in virtualization technology and has the most market share than anyone else. The 2nd in line is Microsoft 
- VMWare virtualization software is called ESXi (hypervisor) 
- VMWare company name is the same as its virtualization software technology
- Other virtualization companies are, Microsoft (Hyper-V), Oracle (OVM/OLVM), Citrix (Xen server), Redhat (KVM), IBM etc.


Availability of the product -->Azure VMware Solution is available in 29 regions only uptill now.
Pricing of the product --> The basic level starts with $8.5/hr and further reduces if the org reserves the instance for 1/3/5 years.
According the sizes available -
- AV36
- AV36P
- AV52
- AV64

Not every type of instance is available in every region as such , and certainly each instance costs different in different regions as well.

## VMware Products

### VMWare Workstation Player 
VMware Workstation Player is an ideal utility for running virtual machines on a Windows, Linux or MAC computers. Organizations use Workstation Player to deliver managed corporate desktops, while students and educators use it for learning and training. The free version is available for non-commercial, personal and home use
### VMWare vSphere Hypervisor or  ESXi
It is said to be a main layer of virtualization i.e ESXi. Bare-metal hypervisor that installs directly onto your physical server.
Now here bare metal means that the hardware does not have its own OS.

### VMWare vSphere Client 
- Is an interface that allows you to connect to a hypervisor 
- It is a client just like RDP for Windows or Putty for Linux servers 
- It is now web-based client (HTML5). The older version was thick client.

### VMWare vCenter
- It’s a management tool to manage multiple hypervisors 
- The interface is same as vSphere client with added functionality.

## Azure VMware solution deployment deep dive

### The five dimension of planning

1. What are you trying to do
2. where are you going to do
3. how much of it do we need
4. how are you going to connect it
5. how are you going to manage it



Decisions Before Planning -->
- do you have any footprint from before?
- is AVS supported in that region?
- if not supported than which is the closest AVS supported region to your existing data center or the user?
- does the customer need AVS private clouds in multiple regions


### Planning for the Size considerations

- determine the initial size and number of clusters and number of host per region required?

## Limits -->
we can have
- ==1 PRIVAT CLOUD PER SUBSCRIPTION==
- ==12 CLUSTERS PER PRIVATE CLOUD==
- ==16 HOSTS PER CLUSTER
- ==96 HOSTS PER PRIVATE CLOUD==

 

## vSphere

- It is known as VMware infratsructure.
- It is collection of all the VMware server virtualization productswhich includes 
  - ESXi hypervisor 
  - vCenter management software

### Features of vSphere 
- VMware ESXi -> its a hypervisor which is used to combine all the components like processors, storage, memory and other resources into multiple virtual machines.
- VMware vCenter server -> also known as virtual center, it is the central control point which provides a single pane of glass view across ESXi hosts.
- VMware vSphere Client -> it is an interface just like RDP or putty which allows the user to remotely access the vCenter server.
- 


# Move on-premises VMware infrastructure to Azure VMWare Solution

For this migration we will use Azure VMware Solution [Azure VMware Solution](https://azure.microsoft.com/services/azure-vmware/)


## Migration goals

An org identifies goals for migrating VMware on-premises to VMware in the cloud:

- Continue to manage org's existing environments by using VMware tools that are familiar to its teams, but also get benefits of azure services.
- Seamlessly move org VMware-based workloads from its datacenter to Azure, and integrate the VMware environment with Azure.
- After migration, the applications in Azure have the same performance capabilities as they do today in VMware. The applications will remain as critical in the cloud as they were on-premises.

So here the Migration goal simply means that the org can  continue to capitalize on its existing VMware investments, experience, and tools, which include VMware vSphere, VMware vSAN, and VMware vCenter Server. But Contoso also gets the scale, performance, and innovation of Azure.



### For onprem Orgs this is the current Architecture

This is current architecture is set up:
- VMs deployed to an on-premises datacenter that's managed through [vSphere](https://www.vmware.com/products/vsphere.html).
- Workloads deployed on a VMware ESXi host cluster that's managed through [vCenter Server](https://www.vmware.com/products/vcenter-server.html), [vSAN](https://www.vmware.com/products/vsan.html), and [VMware NSX](https://www.vmware.com/products/nsx.html).


### Proposed Architecture for the on-prem Org

Org must complete these steps:
- Deploy an [Azure VMware Solution private cloud](https://learn.microsoft.com/en-us/azure/azure-vmware/concepts-private-clouds-clusters) to the Azure regions close to the working place of the org.
- Connect the on-premises datacenter to Azure VMware Solution and run in the Azure region by using virtual networks and [Azure ExpressRoute](https://learn.microsoft.com/en-us/azure/azure-vmware/concepts-networking) with the Global Reach option enabled.
- Migrate VMs to dedicated Azure VMware Solution by using [VMware HCX](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html).

## Migration process

Org will move its VMs to Azure VMware Solution by using the VMware HCX tool. The VMs will run in an Azure VMware Solution private cloud.

To complete the process, the Contoso team takes these steps:

- Plans its networking in Azure and ExpressRoute.
- Creates the Azure VMware Solution private cloud by using the Azure portal.
- Configures the network to include the ExpressRoute circuits.
- Configures the HCX components to connect its on-premises vSphere environment to the Azure VMware Solution private cloud.
- Replicates the VMs, and then moves the VMs to Azure by using VMware HCX.

Now here VMware HCX has many migration methods such that cold migration, replication- assisted vMotion(RAV), or it can be bulk migration.

# creating AVS in AZure Portal

- Go to portal and search Azure vmware solution
- go to it and click **create**
- Fill the details such that 
  - subscription
  - resource grp
  - Name resource
  - Location or Region
  - Size of the host - Select the size accordingly **AV36**, **AV36P** or **AV52** SKU.
  - Host Location - For standard private cloud select 1 availability zone
  - Number of hosts - by default or minimum 3 hosts are req. but it can be increased till 16 through portal and further can be increased by requesting a quota. 
  - Address block - here create the network address block with /22 CIDR because it is further as cluster managed services, which will require large number of IPs. Keep in mind that there should be no conflict between the virtual network IP in azure and On-prem.

![[Pasted image 20240320064323.png]]

- verify all the details then click  ==CREATE==

==It will take almost 3-4 hrs to finish the deployement.==
After completion please verify the status as ==**succeeded**==

## Now next we connect it to on-prem through express route

- go to portal and search express route and the org should have their existing on-prem
- 