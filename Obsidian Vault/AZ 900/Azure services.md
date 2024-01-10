
### Azure functions:

Its a service in azure which comes under PAAS.
In this service the user only works on the functions or set of code required for the application , and rest all the infra is taken care by the azure.

The azure functions service runs on the pay as u go model such that whenever the action or the code is triggered the service is started and cost is calculated for that time of use.

Function can be stateless or statefull

Stateless-- when ever they are triggered the function starts all over again.

Statefull -- when the fuction use is done and started again to continue then a context is passed through a function to track prior activity note.

**2.** **Virtual Machine**
Theese r basically same as the physical machine consisting of cpu and ram etc. but this machine willl be whole virtually all the infrastructure of the machine will be provided by the cloud.
The user can create VM according to their own need, like if they want more storage and less connectivity or may be location should be definite then its all possible.

Vm are basically used to development , testing ,or any other minor tasks.
VM can scalable and highly available,by using some features of azures like availability sets.availability zones,etc.

**VM scale set--**
VMSS helps us to maintain grp of identical vms in a network.
vmss is basically used when large number of vm are required to handle the load on the server.Their comes the role of a LOAD BALANCER which is conncetdd to the all the vms such that when a vm gets over load then  the load balancer will share the load to another vm.
these features happen automatically in vmss it is all handled in back by vmss.

**VM availability set**
VM AS is tool of azure which help us to build more  resilient, highly available environment.
it ensures that when there is a update started in the vm then the performance should not decrease such that other vm should handle the process at that time.
it also ensures that when there is a failure or fault occured in the environment or on the vm then the working should not be interrupted such there is fault domain which takes care of that.

Update domain--> one update should at a time .by that time the other vm comes into play.

fault domain--> the fault domains connect the vm to one single power source and network switch.there will 3 fault domains in a given availability set.
and there can be total of 20 update domains.

WHen to use VM--: 

1.during testing and development
2.to run application on the cloud
3.to start ur datcentre to cloud
4.for disaster recovery

**3.AZURE VIRTUAL DESKTOP**
a virtual desktp is type of a virtual machine which is basically used to run application.
AVD is a service by cloud which is like a desktop and application virtualisation service.

AZure VD works like a usre once make his desktop on the cloud then when ever the user trys to access the desktop it will start with any of the vms availabkle in backend.

AVD allows us to use multisession windws on single vm.

**4.CONTAINERS**
SO basically what happens in vms if we want to run more number of task cincurrently at a time then more vms will be required to that task and more vm means more space and more latency.

# "containers are used to create solutions using a microservice architecture"

to a solution to it its better to start with containers ,now containers make the packet of different operations that has to be taken place and then every task is performed at the same time in the same vm.

AZURE CONTSINER INSTANCE-->
it gives the fastest and safest way to run container in azure, no need to maneg any virtual machine or adopt any other services.
they provide PAAS services

![[Pasted image 20231102190053.png]]

![[Pasted image 20231102125324.png]]


Unlike in vm for running each application personal os are required to run , but in container all the conatiners acn run on the single os that is host OS . for running the conatiner there is a requirement of the container engine and multiple appllication can run on the single host without affecting the performance.


Azure kubernetes services
a container orchestration srvices for managing large number of containers.  
AKS is a complete orchestration services working on highly distributed and  automating and managing large number of contiainers. 