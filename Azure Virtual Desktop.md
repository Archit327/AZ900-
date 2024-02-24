
### What Is Azure Virtual Desktop?

Azure Virtual Desktop is a cloud-based virtual machine solution, which allows you to remotely connect to a virtual desktop from anywhere. All you must do is enter the virtual desktop’s server into any remote desktop tool, and you are ready to go. You can even use Window’s built-in ‘Remote Desktop Connection’ tool!

As AVD is a Microsoft product, it is the best way to run a Windows virtual machine and has the added advantage of being cloud-based — which means great scalability and adjustability compared to traditional on-prem virtual machines.

It also works directly with other Microsoft Azure and 365 products, making it much better for your workflow than any other solution.


**use case 1- Workload placed online**
--> Rather than having to carry your files on a physical computer or hard drive, Azure Virtual Desktop allows you to access your work computer system from any computer, meaning that your files never leave the workplace — but you can work on them anywhere.

And as remote teams grow, and more resources are deployed — Azure Virtual Desktop’s scalability is second to none. You can onboard users easily and can also ensure that leaving employees’ work drives are archived or deleted without having to physically access their computers.

**USECASE 2- Health care data security**
--> The needs of the healthcare industry are enormous due to the importance of providers’ work and the amount of sensitive information that healthcare institutions carry. Azure Virtual Desktop is far more secure than any other solution and allows for data to be stored on encrypted drives that cannot be accessed by physical attacks.

Also, AVD allows multiple users to access the same desktop — which lets healthcare employees have anonymous access to the files that they need whenever they need them without security fears.


**USECASE 3 -- law firms**
--> no requirement of keeping the data with them as it is very sensitive and theft prone, they can simply keep it on avd.

they need workspace where they will keep the application groups for interaction. workspace is like logical grouping of application grps.
### Questionare for CAF for AVD:-

1. Which type of OS windows 10 11,  ?
2. how many host pools and how many session host in one pool?
3. what about the workspace , how to manage the desktops?
4. which type of host pool
	   personal host pool - usually recommended with non- multi session OS i.e windows 7/8/10
	   pooled host pool - usually recommended with windows 10 OS, connections of user are distributed across session hosts. good for average workload
5. Type of load balancing 
	   **breadth first load balancing** -- here the connections are divided to all session hosts equally.
	   ![[Pasted image 20240223221938.png]]
	   
	**depth first load balancing** -- here the new connections are send to the sessions host until the capacity or limit is filled , as the limit of that session host is filled then the new connections are send to next session host. 
	![[Pasted image 20240223222037.png]]


	   