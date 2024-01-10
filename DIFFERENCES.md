
#### Bandwidth vs throughput
* it is the maximum amount of data transmitted over a network or connection .
	  $ throughput is the amount of data transmitted over a network 
* it is a theoretical measurement.
	  $ it is practical measurement
* expressed in bits per sec (bps) . 
	  $ it is measured in BYTES per sec(Bps)
* it indicates the network performance .
	 $ it determines how much data is transmitted
* bandwidth allow us to estimate that how long will it take to receive a certain piece of info .
	  $ throughput measures the number of packets successfully received at the destination


#### NSG vs ASG

NSG and ASG are the main azure resources that are used to administrate and control network traffic within vnet


NSG is used to controll the network traffic within the vnet
whereas ASG is an object reference within that NSG 

NSG will allow or deny the access into the network in many ways
* communication between diff workload on a vnet
* network connectivity from onsite to environment into azure
* direct internet connection

ASG are used within an NSG to apply a NSG rule to spcific workload or may be grp of VMs , the rule will given by ASG to those vms working as network object and explict ip address are added to this object 

if we create new vm and provide it with same asg rule then the vm will get all the nsg rules in place for that specific ASG. 

![[Pasted image 20231117134040.png]]



| AZURE BLUEPRINT                                                                          | TERRAFORM                                                              |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| limited to azure                                                                         | all cloud platform                                                     |
| gui thats why easy to use                                                                | hashicorp configuration lang it is complex to configure and setup      |
| governance and compliance                                                                | null                                                                   |
| top to down approach i.e from subs level to resource level                               | bottom to up i.e start from deploying resource level to subs level     |
| it is for azure only so it provide security , availability                               | for multiple cloud platform ,so not that secure                        |
| we can define the blueprint and then assign it                                           | we can modules in the same way                                         |
| manage nd organise azure resources, rbac , policy,                                       | manage compute , storage, network resources on the multicloud platform |
| it is cost free to use but the cost for underlying resources will be varying accordingly | its basic level is free but                                                                        |







