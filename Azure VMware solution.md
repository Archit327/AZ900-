

- VMware, Inc. is an American cloud computing and virtualization technology company headquartered in California. 
- VMware was the first commercially successful company to virtualize the x86 architecture. (Wikipedia) 
- VMWare is the leader in virtualization technology and has the most market share than anyone else. The 2nd in line is Microsoft 
- VMWare virtualization software is called ESXi (hypervisor) 
- VMWare company name is the same as its virtualization software technology
- Other virtualization companies are, Microsoft (Hyper-V), Oracle (OVM/OLVM), Citrix (Xen server), Redhat (KVM), IBM etc.



## VMware Products

### VMWare Workstation Player 
VMware Workstation Player is an ideal utility for running virtual machines on a Windows, Linux or MAC computers. Organizations use Workstation Player to deliver managed corporate desktops, while students and educators use it for learning and training. The free version is available for non-commercial, personal and home use
### VMWare vSphere Hypervisor or  ESXi
It is said to be a main layer of virtualization i.e ESXi. Bare-metal hypervisor that installs directly onto your physical server.
Now here bare meta means that the hardware does not have its own OS.

### VMWare vSphere Client 
- Is an interface that allows you to connect to a hypervisor 
- It is a client just like RDP for Windows or Putty for Linux servers 
- It is now web-based client (HTML5). The older version was thick client.

### VMWare vCenter
- It’s a management tool to manage multiple hypervisors 
- The interface is same as vSphere client with added functionality.

## Azure vmware solution deployment deep dive

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

 

