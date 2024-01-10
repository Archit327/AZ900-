[[Load Balancer Task]]
%%TOPICS TO BE COVERED
proxy and reverse proxy?
concept used in load balancer?
round robin ,priority ,weighted ,availability
hash based load balancing
onpremises FL load balnacing?
choosing the correct load balancing?
recursive dns?
limitations of load balancer? https://learn.microsoft.com/en-in/azure/load-balancer/components#limitations
components of load balncer?[Azure Load Balancer components | Microsoft Learn](https://learn.microsoft.com/en-in/azure/load-balancer/components#limitations)

create public lb and private lb

web application firewall? %%

Proxy vs Reverse Proxy
=
![[Pasted image 20231213195028.png]]

Proxy server
 Basically when there is private network or may be globally a machine wants to access the server through the internet then they deploy a proxy server which will go out on the internet and carry the task on that computers behalf.

Reverse Proxy Server
  When the load balancer is placed in the site of backend that is it is connected to the server side where all the request comee to, there the balancer balances the requests to the different server according to their capabilities or their strength or their current requests holdings.


Load balancer
=
Load balancer is efficiently distributing the incoming traffic to the group of backend server accordingly.
Load balancer works on layer 4 OSI model.

when there is a incoming traffic to back end and it there are numerous number of vms server for handling the traffic then the load balancer will distribute the traffic accordingly to the server who are available or can handle that kind of heavy load.


Types of load balancers table-->

| Service                   | Global/Regional    | Recommended traffic | location |     |
| ------------------------- | ------------------ | ------------------- | -------- | --- |
| Azure Front Door          | Global             | HTTP(S)             | not req  |  ip based   |
| Azure Traffic Manager     | Global             | Non-HTTP(S)         | not req  | dns based    | 
| Azure Application Gateway | Regional           | HTTP(S)             | requ     |   ip based  |
| Azure Load Balancer       | Regional or Global | Non-HTTP(S)         | required |   ip based  |


the **rules** in load balancing:
1. NAT rule
2. LOAD BALNCING RULE
3. HA PORT
4. OUTBOUND

HEALTH PROBE->
LB requires a health probe to get the status of the backend server.
it can determine that is the backend available and healthy or gets failover and has some functional problem.
And if the health probe fails then the LB stops sending the connections and load to that respective server.


The health probe checks the health of the backend pool instances and if health probe fails then it mark it as unhealthy and LB would not send the traffic to that instance .
the health will check the instance after every 5 sec by default the time can be changed accordingly as the health probe succeeds for that instance it gets updated to healthy and again LB can send traffic to that instance. 


Floating IP
=

Floating IP is brought into consideration if we want to use the same port and IP for many different instances.

 So baiscally Floating IP works in the concept of DSR(Direct Server Return) which means sending the results directly to the client
For example in load balancing when we enable the floating ip then when a connection reaches to the server then the ip is shown as FrontendIP of the load balancer with the port , this happens so that when the server finds the result and wants send back the result then it directly send it to the client which reduces the time effort of the LB.

if floating ip is not enabled then the server  who gets the reequest will get its ip destination ip and port and then it has to send the result to that ip after which LB again configures and sends the result to the ip or the client from where it was received.


![[Pasted image 20231227162741.png]]


