
step 1
![[Pasted image 20231121151218.png]]

Step 2
![[Pasted image 20231120190850.png]]

![[Pasted image 20231120193803.png]]

![[Pasted image 20231120193825.png]]

![[Pasted image 20231120193933.png]]

![[Pasted image 20231120193938.png]]


![[Pasted image 20231121150205.png]]

![[Pasted image 20231121150254.png]]

![[Pasted image 20231121150259.png]]



![[Pasted image 20231120194221.png]]

![[Pasted image 20231120194247.png]]

![[Pasted image 20231120194308.png]]


![[Pasted image 20231120194349.png]]

![[Pasted image 20231120194402.png]]


VM1(vnet1) connected  to VM2 (vnet3)

![[Pasted image 20231121131808.png]]


POINT TO SITE CONNECTION VM1 to LOCAL MACHINE


![[Pasted image 20231121143224.png]]



![[Pasted image 20231121144451.png]]



Referred Linkss :

For Point 2 Site configuration--> 
[About Azure Point-to-Site VPN connections - Azure VPN Gateway | Microsoft Learn](https://learn.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about)

For VNET 2 VNET -->
[Configure a VNet-to-VNet VPN gateway connection: Azure portal - Azure VPN Gateway | Microsoft Learn](https://learn.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)


Tutorial: Create and manage a VPN gateway using the Azure portal -->
[Tutorial – Create & manage a VPN gateway – Azure portal - Azure VPN Gateway | Microsoft Learn](https://learn.microsoft.com/en-us/azure/vpn-gateway/tutorial-create-gateway-portal)




# point 2 site via azure entra authentication

downloaded  AZURE VPN CLIENT and connected the vnet  


#### CONFIGURATION FOR AZURE ACTIVE DIRECTORY 
![[Pasted image 20231121175150.png]]

![[Pasted image 20231121182710.png]]


After changing the configuration downloaded the vpn client file and imported in the azure vpn client  and then connected to vnet.

VM1 to VM2
VM1 TO LOCAL MACHINE
VM2 TO VM1
VM2 TO LOCAL MACHINE
![[Pasted image 20231121174820.png]]


LOCAL MACHINE TO VM1
LOCAL MACHINE TO VM2

![[Pasted image 20231121174903.png]]




