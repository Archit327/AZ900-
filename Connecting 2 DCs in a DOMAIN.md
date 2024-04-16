
![[Pasted image 20240416090117.png]]


![[Pasted image 20240416091744.png]]

![[Pasted image 20240416102200.png]]
Powershell Command for seeing the roles given to which DC
--> ==netdom query fsmo==

Powershell command for changing the role of the DC
---> 
Move-ADDirectoryServerOperationMasterRole -Identity "<Name of the DC to which you want to transfer the role >" -OperationMasterRole <Name of the role>

![[Pasted image 20240416110028.png]]