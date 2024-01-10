
#### Protocol
A defined set of standard that computers must follow in order to communicate properly.

#### computer networking
the name given to the full scope of HOW COMPUTERS COMMUNICATE WITH EACH OTHER.


### How Does a Computer Network Work?

Basic of computer networking is relied on two things 
### NODES and LINKS

#### NODES 
* a network node can be illustrated as equipment for data communication like modem or router etc. or may be equipment which is required to connect two or more computers.

### LINKS
* these r like the wires or the cables are the links between the two nodes makin a connection.


### BASIC TERMINOLOGIES OF NETWORKING

- **IP Address**: An IP address is a unique numerical identifier that is assigned to every device on a network. IP addresses are used to identify devices and enable communication between them.
- **DNS:** The  is a protocol that is used to translate human-readable domain names into IP addresses that computers can understand.
- **Firewall:** A firewall  is a security device that is used to monitor and control incoming and outgoing network traffic. Firewalls are used to protect networks from unauthorized access and other security threats.


#### NETWORK DEVICES

THe devices like hubs, routers, switches etc which can be used as a medium to communicate between two different devices, these r known as network devices.

#### OSI MODEL

**O**pen **S**ystem **I**nterconnection is refernce model that specifies standards for communication protocol and also the functionalities of each layer.

There r 7 layer in OSI model-->
1. physical 
2. data link layer
3. network layer
4. transport
5. session
6. presentation
7. application

#### MAC ADDRESS
Media Access Control Address is also known as a physical address and is unique and given to the each host. and is associated with the NIC(Network Interface Card).
MAC address is assigned to the NIC at any time of manufacturing
the length of the MAC address is of 6bytes or 48  bits.


**Port:** 
A port can be referred to as a logical channel through which data can be sent/received to an application. Any host may have multiple applications running, and each of these applications is identified using the port number on which they are running.

A port number is a 16-bit integer, hence, we have 216 ports available which are categorized as shown below: 

| **Port Types**       | **Range**     |
| ---------------- | ------------- |
| Well known Ports | 0 – 1023      |
| Registered Ports | 1024 – 49151  |
| Ephemeral Ports  | 49152 – 65535 |


**Socket:** 
The unique combination of IP address and Port number together is termed a Socket.


**ARP:** 
stands for **Address Resolution Protocol**. It is used to convert an IP address to its corresponding physical address(i.e., MAC Address). ARP is used by the Data Link Layer to identify the MAC address of the Receiver’s machine.

**RARP:**
 stands for **Reverse Address Resolution Protocol**. As the name suggests, it provides the IP address of the device given a physical address as input. But RARP has become obsolete since the time DHCP has come into the picture.



### HUBS
It is a normal connection device which is has its no extra feature involved the  just sharing the data came to one port to evry port in that hub.
* hubs are not intelligent as it cannect identify IP and MAC 
* they just check that a device is connected to the port and further when  packet i arrived then that is shared to each port and devices connected to that port.
* hubs work on broadcast always, i.e if a data packet arrives to the port of the hub it just does the broadcast and send it to eevry port , which depicts no privacy  

#### SWITCH
Switches r same as hubs , it also has multiple port to connect devices but its better than the hubs such that 
* it uses the broadcast and unicast feature 
* it manage the MAC table
* it improves security and privacy within network by sending the data packet only to the destination 


#### modem
* Mode is what that brings the internet into your home or business.
* Modem converts the analog signal coming from the ineternet to the digital signal that r understood by the computer and vice versa. 

#### Router

TO exchange data within the network hubs and router can be used but if we want to exchange data outside the network , a device needs to be able to read IP address.
So router is used to route data from one network to another based o their IP address. 

* a router is a device that routes or passess your internet connection to all your devices.its basically like a device which will get connected to multiple devices and provide the access to the internet.                                    
* for example in home in wifi once it is established and connected to internet , it can be connected to multiple devices in the home and provide the internet accessibility.
* actually wifi router is a modern router which contains the capability of both router and modem as well as switch.
* router can be connected to devices through wireless as well as wired connectivity, with help of ethernet cables.  
*  when a data is recieved by router it checks the IP of the data packet and determines does the data belong to own network or another network .
* so the router is basically the GATEWAY of the network.

#### WIFI ROUTER / WAP

| WIFI ROUTER                                                                      | WAP                                                                                                                                                 |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| these are mainly used in home and small offices                                  | and these are required for large offices                                                                                                            |                                                                           | -                                                                                                                                                   |
| it connect directly to modem and provides the internet connection to the devices | they cannot connect directly to the modem they dont have any port to connect to the modem                                                           |                                                                            | -                                                                                                                                                   |
| it can connect to dives by wire or wireless                                      | WAP can be only connected wireless , they will be first connected to wifi router nd then provide the internet connection to devices connected to it |                                                                              | -                                                                                                                                                   |
| it has inbuilt DHCP which will assign a IP to the the devces connected           | it doesnt hav DHCP but it will inherit the IP from the wifi router and then asign to the devce connected                                            |                                                                           |                                                                                                                                                     |
| it has firewall                                                                  | it doesnt have firewall                                                                                                                                                    |

the advantage of using WAP
* if hav to make changes in one wifi then u hav to make it once and it will be automatically managed for WAP.


#### CABLE 
is a high speed access technology that uses cable modem with an atteched coaxial cable which provide a link to the internet service provider.
these broadand cable are same as televison cable .
these television broadband provider often give u a modem which has the features of a switch and wifi router and modem.
and to connect it we require a coaxial cable through that cable we will be able access the internet which is coming to modem.
the basic disadvantage of the cable is that single cable is connected by everyone in ur neighbour and hence the delivery speed of the internet gets decreased. 

#### digital subscriber line(DSL)
it is also used to access the broadband over the internet
* it can carry both voice and data at the same time
* it is not as fast as compared to cable
* everyone has their own bandwidth
* it will be connected to a typical phoneline at the back of the modem. 
* **ADSL** --> Asymmetric Digital Subscriber Line 
  it has download speed faster than the upload speed
* **SDSL*** --> Symmetric Digital Subscriber LIne 
  it has download and upload speed same
* **VDSL*** --> very hight bit DSL 
  beacause it uses copper wires and only made for short distance


#### fiber Cable 
IT is the fastest internet speed available today .
it has upload and download speed of 1000MBps
it uses light to send dat
due to light effect of sending fiber can have longer connection


#### ETHERNET CABLES

1. Twisted pair cable (without shield)
2. Twisted pair cable (with shield)

these cables r atteached to RJ45 to connect to the devices



twisted pair cable consists of different types of coloured wires twisted around each other and in basically 2 different combination

![[Pasted image 20231107164706.png]]


**STRAIGHT CABLE**
if both the ends of the cable are of same standard then it is a straight cable
it is used for LAN
if it is used to connect devices in small network may be with switches and router and hubs etc 
it is used to coonect two disimilar devices together 
**CROSSOVER CABLES**
it both the ends of the cable are of different standard the it is a crossover cable
it is used to connect two similar devices together
like hubs to hub and computer to computer

![[Pasted image 20231107171006.png]]

Categories of twisted pair cable

![[Pasted image 20231107171234.png]]![[Pasted image 20231107171255.png]]


 **TYPES OF PVC RATED CABLES USED IN CONNECTION IN OFFICES**
![[Pasted image 20231107174031.png]]


  
**TYPES OF CONNECTORS**
1. RJ-45 connector--> it is a 8 pin cable connector
2. UTP Coupler-->  it is used to connect the RJ-45 connector at both side side to increase the length
3. RJ-48 connector--> it is same as RJ-45 , the difference is that it has a axtra shield that covers the wire.
4.  F-TYPE connector--> this connector is basically used on a coaxial cable
   primarily used by cable provider to attach to cable modems.
5. USB connector - Universal Service Bus is a connecror which is a multi purpose connector including internet , charging , sharing of data etc.
6. FIBER OPTIC connector --> they use pulse of light to send data.
   1. SC connector--> called standard connector or square connector
      it is push pull connector to audio and video plugs.
    2. MTRJ connector--> it is fiber optic connector  which has high packed density and are designed to replace the SC connector . The MTRJ uses two fibres in single design, and it looks like a RJ-45. 


### DNS (Domain Name System)
* it is a directory service that provides a mapping betwwen the name of the host on the network and its numerical address.
* it is required for functioning of the internet.
* DNS is a service that translate the domain name into ip address. this allows the user of network to utilize the user friendly name of the host  rather than using the or remembering the IP address of the host site.
* DNS just works like a phone book to find a number u find the name of the person and then we get the number,same in DNS u provide the name of the site to the browser then browser will go to DNS server and sesrch for that name and finally finding the name the dns server will get back the IP address atteched to that name and after getting that ip we can reach to that website and browse.  

![[Pasted image 20231107204215.png]]

#### Working of DNS 
**Step1** 
Firstly the user will go on browser and search for any site by typing the site name eg- Yahoo.com
now secondly the typed host name will be searched in the cache memory of the user's device
if its not found it will go to the "resolver"

**STEP2** 
now the **RESOLVER** is like INternet service provider and it will aslo search for the host nam ein its cache memory , if its not found then it will go the higher authority where all the server knowledge is present
everytime a resolver gets to know about any IP address of the site the user asked for it store it in its cache memory so that in future if the user again wants to access the same site then it will be easier to directly send the ip by uts own rather than asking for higher authority

**STEP3**
now at the **ROOT SERVER** level the host name is identified such what is the index name in it like .com , .in , .org etc
ROOT SERVER doesnt contain the data about the ip address but it knows where that ip can be found of that given domain name,
it is basically serves as a refernce to more specific location.
it will then send it to the **TLD** 

**STEP4** 
**TLD** the top level domain server can be thought of as a specific section where all the grouped indexing domain will be present .
TLD hosts the last name of the host name(.com)
the TLD stores the address info for the top level domains.
TLD also doesnt conatin the IP address 
TLD will direct the resolver to the last level i.e the **AUTHORITATIVE NAME SERVER** 

**STEP5** 
no here the resolver will ask the Authoritative name server about the ip address of the given domain name or host name
**AUTHORITATIVE NAME SERVER** are responsible for knowing everything about the domain
including the IP address
here the resolver will get the IP of the domain name provided  from **AUTHORITATIVE NAME SERVER** 

**END**
at the end the resolver will send the computer the ip address of the given domain name and now u can access the website with help of the IP address.

#### DNS RECORD
What is a DNS Record?
-->DNS record are instruction or record files which contains the information about the    domain, including its IP address and how to handle the request for that domain.
-->these record consist of a series of text file written in DNA sysntax.
-->all DNS records have TTL-(Time To Live) , which indicates that how much time will it take to refresh that record.
##### 1. A record
The record that holds the IPv4 address of a domain 

##### 2. AAAA record
The record contains the IPv6 address for domain

##### 3. CNAME record
forwards one domain or subdomain to another domain, doesnt provide IP address

##### 4. MX record 
directs mail to email server, connected to that domain.

##### 5. TXT record
lets an admin store text notes in the record. these records are often used for email security.

##### 6. NS record 
store the name server for DNS entry.

##### 7. SOA record 
Start of authority
store admin information about a domain.

##### 8. SRV record 
specifies a port for specific services.

##### 9. PTR record 
provides a domain name in reverse lookups.


#### DHCP(Dynamic Host Configuration Protocol)

* It is a network management protocol used on IP networks.
* it is a server which assigns an IP address and other configurations to the connected devices on the network to communicate with others.
* without dhcp the IPs for a computer are manually assigned , but later these devices cannot be connected outside the local subnet.
* router can be treated as dhcp server
* it provides ip address to the devices connected on a lease i.e after a given time the accesssibility will expire and u have to renew the lease and regain ip .
* we can also create  a reservation for the ip address for our devices , like router , server printer, because devices like these should be given IP address constantly.
* it is server which uns on a server like microsoft server etc.
* router have inbuilt dhcp server N




### Components of DHCP

Let's understand it's components to get more about the DHCP server. The DHCP server's components are as following:

**DHCP server:** It is a networked device that allows us to run the DHCP service, which keeps IP address and other configurations. Usually, it is a router or server, but it could be anything that acts as a host, for example, an SD-WAN appliance.

**DHCP client:** The DHCP client is an endpoint that receives data from the server. It can be a mobile device, computer, or anything else that is connected to the network. Most of the devices are configured to obtain the DHCP data by default.

**IP address pool:** It is a range of IP addresses available to clients. Generally, it is handled in ascending order.

**Subnet:** The subnet is the partition of the IP networks into a small segment. It allows easy management of the IP addresses.

**Lease:** The Lease is the length of the time taken by the DHCP client to hold the information. Once a lease is expired, the client will have to renew it.

**DHCP relay:** The relay is a router or host that receives the client's messages being broadcasted over the network and forwards it to a configured server. The server responds to the relay. It is useful for centralizing a DHCP server



### IP Address
An IP address is a number identifying of a computer or another device on the Internet.
Internet Protocol address is the address assigned to a device which want to communicate to other device  within a network or may if it wants to connect to internet outside the network.
IP address is 32 bits address divided in 4 section 
### **---.---.---.---**
8b .  8b  . 8b . 8b

every 8bit can make upto 256 value
such that an ip can start from **0.0.0.0** and go upto **256.256.256.256**
There are two type of IP address 
* public 
* private

public are not personally given to any devices , when a device want to connect to internet or to other device then it is provided with Public IP.
* for providing the ip here comes the expertise of a router or the ISP .

Private ip can be given personally to the devices but they are also less in number , they r also given by ISP or the wifi router.

### Range of IP
The ip range are particularly divided into parts defining that how many number of networks will be available and how they can be assigned to the  devices.
* In class A only 8 bits are taken for network bits rest 24 for the host network
* In class B only 16 bits are taken for network bits rest 16 for the host network
* In class C only 24 bits are taken for network bits rest 8 for the host network
- Class D addresses are 32-bit network addresses. All the values within the range are used to identify multicast groups uniquely.
- Class E IP address is defined by including the starting four network address bits as 1.



|Class|Address Range|Subnet masking|Example IP|Leading bits|Max number of networks|Application|
|---|---|---|---|---|---|---|
|IP Class A|1 to 126|255.0.0.0|1.1.1.1|8|128|Used for large number of hosts.|
|IP Class B|128 to 191|255.255.0.0|128.1.1.1|16|16384|Used for medium size network.|
|IP Class C|192 to 223|255.255.255.0|192.1.11.|24|2097157|Used for local area network.|
|IP Class D|224 to 239|NA|NA|NA|NA|Reserve for multi-tasking.|
|IP Class E|240 to 254|NA|NA|NA|NA|This class is reserved for research and Development Purposes.|

The number 127 is reserved for loopback, which is used for internal testing on the local machine.
### Private IP Range
Private IP address exists within the specific ranges as reserved by the Internet Assigned Numbers Authority (IANA). Following are the address ranges of private IP addresses:

- Class A: 10.0.0.0 to 10.255.255.255
- Class B: 172.16.0.0 to 172.31.255.255
- Class C: 192.168.0.0 to 192.168.255.255


| Public IP Range | Public IP Range                | Private IP                        | # of Networks | # of Hosts per Network |            |  |     |
| --------------- | ------------------------------- | ----------------------------------- | ------------- | ---------------------- | ---------- |
| Class A         | 1.0.0.0 to  <br>127.0.0.0       | 10.0.0.0 to  <br>10.255.255.255     | 255.0.0.0     | 126                    | 16,777,214 |  
| Class B         | 128.0.0.0 to  <br>191.255.0.0   | 172.16.0.0 to  <br>172.31.255.255   | 255.255.0.0   | 16,382                 | 65,534     |    
| Class C         | 192.0.0.0 to  <br>223.255.255.0 | 192.168.0.0 to  <br>192.168.255.255 | 255.255.255.0 | 2,097,150              | 254        |     

## Special IP Addresses

- IP Range: 127.0.0.1 to 127.255.255.255 are network testing addresses (also referred to as loop-back addresses). These are virtual IP address, in that they cannot be assigned to a device. Specifically, the IP 127.0.0.1 is often used to troubleshoot network connectivity issues using the ping command.


![[Pasted image 20231108162912.png]]


#### APIPA
- _Automatic Private IP Addressing_ (APIPA) is a feature with _Microsoft Windows_-based computers to automatically assign itself an IP address within this range if a _Dynamic Host Configuration Protocol_ (DHCP) server is not available on the network. 
- A DHCP server is a network device that is responsible for assigning IP addresses to devices on the network.   
- At your home, your Internet modem or router likely provides this functionality. In your work place, a _Microsoft Windows Server_, a network firewall, or some other specialized network device likely provides this functionality for the computer at your work environment.
- Class B Private APIPA Range: 169.254.0.0 to 169.254.255.255
- with subnet mask of 255.255.0.0

### MAC address
Media Access Control address is an identifier that every network device uses to uniquely identify itself on a network. 
***So no two devices in the world will have same MAC address***
it is made up of ***6 bytes hexa decimal number***  that is carved into every NIC 
for example-->
						**00-04-5A-63-41-66**
The starting 3 bytes identify the manufacturer of the NIC, such that TP-LINK, Netgear etc 

