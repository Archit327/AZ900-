Demoting a Domain Controller (DC) involves removing it from the Active Directory (AD) domain, effectively turning it into a standalone server or a member server in a workgroup. This process is useful when you no longer need a server to function as a DC, or when decommissioning or replacing hardware. Before proceeding, ensure that the DC you plan to demote is not hosting any critical roles like FSMO roles unless those roles have been transferred to another DC.

### **Pre-requisites and Considerations**

1. **Backup**: Ensure you have a recent backup of the DC and the AD environment.
2. **Transfer FSMO Roles**: Ensure that any Flexible Single Master Operations (FSMO) roles held by the DC are transferred to another DC before demotion.
3. **Replication**: Verify that all changes have been replicated to other DCs to avoid any data loss.
4. **DNS**: Ensure that the server is not the sole DNS server for the domain.
5. **Communication**: Notify relevant stakeholders of the planned demotion and any potential impacts.

### **Step-by-Step Guide to Demote a Domain Controller (DC) Using Server Manager**

#### Step 1: Open Server Manager

1. Log in to the Domain Controller with administrative privileges.
2. Open **Server Manager** from the Start menu.

#### Step 2: Access the Active Directory Domain Services Role

1. In **Server Manager**, click on **Manage** in the top-right corner and select **Remove Roles and Features**.
2. In the **Remove Roles and Features Wizard**, click **Next** until you reach the **Select server roles** page.

#### Step 3: Begin Demotion Process

1. Uncheck **Active Directory Domain Services**.
2. A pop-up will appear asking if you want to remove the features associated with AD DS. Click **Remove Features**.
3. Continue through the wizard by clicking **Next** until you reach the **Remove Features** confirmation page.
4. Check the box that says **Demote this domain controller**.
5. Click **Next** to proceed to the **Active Directory Domain Services Configuration Wizard**.

#### Step 4: Demote the Domain Controller

1. **Force the Removal (if necessary)**: If the DC is the last domain controller in the domain or is experiencing connectivity issues, check the box **Force the removal of this domain controller**. Otherwise, proceed without forcing.
2. **Credentials**: Enter the credentials for a user account that has permissions to demote the DC.
3. **Warnings**: The wizard will display warnings regarding roles and services that will be affected. Review these carefully.
4. **DNS Delegation**: Choose whether to **Remove DNS delegation** if the DC is also a DNS server.
5. **Final Confirmation**: Click **Next** to review the demotion options and then click **Demote**.

#### Step 5: Complete the Demotion

1. The server will automatically restart after the demotion process is complete. You can choose to manually restart later by unchecking the **Reboot on completion** option, though this is not recommended.
2. Click **Demote** to start the demotion process.

#### Step 6: Post-Demotion Tasks

1. **Verify Demotion**: After the server restarts, log in and check that the server is now either a member server or a standalone server, depending on the demotion choices.
2. **Check Replication**: Verify that replication to other domain controllers is functioning correctly using tools like `repadmin` or `dcdiag`.
3. **Clean Up Metadata** (if necessary): If the demotion was not clean (due to network issues or other problems), you might need to clean up the AD DS metadata using `ntdsutil` or Active Directory Users and Computers.
4. **Remove from Domain**: If required, disjoin the server from the domain by going to **System Properties** > **Computer Name** > **Change**, and selecting **Workgroup**.

### **Demotion via PowerShell**

Alternatively, you can demote a DC using PowerShell:

1. Open **PowerShell** as an administrator.
2. Run the following command:

   ```powershell
   Uninstall-ADDSDomainController -DemoteOperationMasterRole -ForceRemoval -RemoveApplicationPartition -Force
   ```

3. The server will demote itself and reboot automatically.

### **Post-Demotion Considerations**

- **FSMO Role Holders**: Ensure that the FSMO roles are correctly assigned to remaining DCs.
- **DNS and DHCP**: Verify DNS and DHCP configurations if these services were running on the demoted DC.
- **Replication Health**: Continue to monitor the replication health across your domain controllers to ensure the environment remains stable.

By following these steps, you can safely and effectively demote a domain controller in your Active Directory environment.


---

