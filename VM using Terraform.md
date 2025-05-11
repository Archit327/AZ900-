To create a Virtual Machine (VM) in Azure using Terraform, join it to a domain, and promote it to a Domain Controller, you need to perform several steps. This includes setting up the necessary Azure resources, deploying a VM, joining it to the existing Active Directory (AD) domain, and then promoting it to a Domain Controller (DC). 

Below is a step-by-step guide to accomplish this:

### **Prerequisites:**

1. **Azure Account**: Ensure you have an Azure account with the necessary permissions to create resources.
2. **Terraform Installed**: Make sure Terraform is installed on your local machine.
3. **Active Directory Domain**: An existing Active Directory domain to which the VM will be joined.
4. **Azure CLI**: Installed and authenticated.

### **Step 1: Define the Terraform Configuration**

First, you need to create a Terraform configuration file (`main.tf`) to define the Azure resources.

```hcl
# main.tf

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "example-resource-group"
  location = "East US"
}

resource "azurerm_virtual_network" "vnet" {
  name                = "example-vnet"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "subnet" {
  name                 = "example-subnet"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.2.0/24"]
}

resource "azurerm_network_interface" "nic" {
  name                = "example-nic"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
  }
}

resource "azurerm_windows_virtual_machine" "vm" {
  name                  = "example-vm"
  location              = azurerm_resource_group.rg.location
  resource_group_name   = azurerm_resource_group.rg.name
  size                  = "Standard_DS1_v2"
  admin_username        = "azureuser"
  admin_password        = "ComplexPassword123!"
  network_interface_ids = [azurerm_network_interface.nic.id]

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "MicrosoftWindowsServer"
    offer     = "WindowsServer"
    sku       = "2019-Datacenter"
    version   = "latest"
  }
}

resource "azurerm_virtual_machine_extension" "domain_join" {
  name                 = "example-domain-join"
  virtual_machine_id   = azurerm_windows_virtual_machine.vm.id
  publisher            = "Microsoft.Compute"
  type                 = "JsonADDomainExtension"
  type_handler_version = "1.3"

  settings = <<SETTINGS
    {
        "Name": "<Your_Domain_Name>",
        "OUPath": "OU=Domain Controllers,DC=example,DC=com",
        "User": "<Domain_Admin_Username>",
        "Restart": "true",
        "Options": "3"
    }
  SETTINGS

  protected_settings = <<PROTECTED_SETTINGS
    {
        "Password": "<Domain_Admin_Password>"
    }
  PROTECTED_SETTINGS
}
```

### **Explanation of the Configuration:**

1. **Resource Group**: A new resource group is created for the VM.
2. **Virtual Network and Subnet**: A virtual network and subnet are defined where the VM will reside.
3. **Network Interface**: A network interface is created and assigned to the VM.
4. **Virtual Machine**: A Windows Server 2019 Datacenter VM is created.
5. **VM Extension for Domain Join**: The `JsonADDomainExtension` is used to join the VM to the domain. You need to replace `<Your_Domain_Name>`, `<Domain_Admin_Username>`, and `<Domain_Admin_Password>` with your actual domain details.

### **Step 2: Apply the Terraform Configuration**

To apply the Terraform configuration:

```bash
# Initialize the configuration
terraform init

# Plan the deployment
terraform plan

# Apply the configuration
terraform apply
```

Confirm the apply with `yes` when prompted.

### **Step 3: Promote the VM to a Domain Controller**

Once the VM is deployed and joined to the domain, you need to promote it to a Domain Controller. This step involves running PowerShell commands on the VM.

1. **Remote into the VM**: Use RDP to access the VM.
2. **Install ADDS Role**: Open PowerShell as an Administrator and run the following command to install the ADDS role:

   ```powershell
   Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
   ```

3. **Promote to Domain Controller**: Run the following command to promote the server to a Domain Controller:

   ```powershell
   Import-Module ADDSDeployment
   Install-ADDSDomainController `
     -InstallDns `
     -Credential (Get-Credential) `
     -DomainName "example.com" `
     -SiteName "Default-First-Site-Name" `
     -DatabasePath "C:\Windows\NTDS" `
     -LogPath "C:\Windows\NTDS" `
     -SysvolPath "C:\Windows\SYSVOL" `
     -NoRebootOnCompletion $false `
     -Force:$true
   ```

   Replace `"example.com"` with your actual domain name.

### **Step 4: Validate the Domain Controller**

After the server restarts, it should be a Domain Controller. You can validate this by:

- Checking the **Active Directory Users and Computers** console to see if the new DC is listed.
- Verifying that **DNS** is properly configured and integrated.

### **Summary**

This guide provides a step-by-step process to create an Azure VM with Terraform, join it to a domain, and promote it to a Domain Controller. This setup is critical for organizations that need to extend their on-premises AD environment to Azure or for those setting up a new AD environment in Azure.   


