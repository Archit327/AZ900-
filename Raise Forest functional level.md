Raising the functional level of your Active Directory (AD) from Windows Server 2012 R2 to Windows Server 2016 involves a few steps to ensure compatibility and seamless upgrade across your environment. The domain functional level (DFL) and forest functional level (FFL) dictate which Active Directory features are available to your environment, and raising them can unlock new features available in Windows Server 2016.

### **Pre-requisites and Considerations**

1. **Ensure All Domain Controllers Are Running Windows Server 2016 or Higher**:
   - Before you can raise the functional levels, all domain controllers in the domain (for DFL) and forest (for FFL) must be running Windows Server 2016 or higher.

2. **Verify Application Compatibility**:
   - Ensure that all applications dependent on Active Directory are compatible with Windows Server 2016.

3. **Backup Your Environment**:
   - Perform a full backup of your Active Directory environment, including system state backups of all domain controllers.

4. **Check Replication and Health Status**:
   - Use tools like `dcdiag` and `repadmin` to check the health of domain controllers and ensure replication is working correctly.

5. **Administrative Privileges**:
   - Ensure you have the necessary administrative privileges to raise the domain and forest functional levels.

### **Steps to Raise the Domain Functional Level (DFL) to Windows Server 2016**

#### Step 1: Open Active Directory Domains and Trusts

1. Log in to a domain controller with a user account that has the necessary permissions.
2. Open **Active Directory Domains and Trusts** from the **Server Manager** or by running `domain.msc` from the **Run** dialog.

#### Step 2: Check Current Domain Functional Level

1. In the **Active Directory Domains and Trusts** console, right-click the domain you want to raise the functional level for (e.g., `example.com`).
2. Select **Properties**.
3. Note the current **Domain functional level**. Ensure it is set to Windows Server 2012 R2.

#### Step 3: Raise the Domain Functional Level

1. Right-click the domain again (e.g., `example.com`) and select **Raise Domain Functional Level**.
2. In the **Raise Domain Functional Level** window, select **Windows Server 2016** from the drop-down list.
3. Click **Raise** to apply the change.
4. Confirm the operation when prompted.

#### Step 4: Verify the New Domain Functional Level

1. After the change has been made, verify that the domain functional level has been updated to **Windows Server 2016** in the **Properties** window of the domain.

### **Steps to Raise the Forest Functional Level (FFL) to Windows Server 2016**

#### Step 1: Open Active Directory Domains and Trusts

1. While still in the **Active Directory Domains and Trusts** console, right-click the **Active Directory Domains and Trusts** node at the top of the left pane.

#### Step 2: Check Current Forest Functional Level

1. Select **Raise Forest Functional Level**.
2. The current **Forest functional level** is displayed. Ensure it is set to Windows Server 2012 R2.

#### Step 3: Raise the Forest Functional Level

1. In the **Raise Forest Functional Level** window, select **Windows Server 2016** from the drop-down list.
2. Click **Raise** to apply the change.
3. Confirm the operation when prompted.

#### Step 4: Verify the New Forest Functional Level

1. After raising the forest functional level, you can verify it in the **Raise Forest Functional Level** window to ensure it now shows **Windows Server 2016**.

### **Post-Upgrade Steps**

1. **Verify Replication**:
   - Ensure that all domain controllers have replicated the changes correctly by using tools like `repadmin` or the **Active Directory Replication Status Tool**.

2. **Check Event Logs**:
   - Review the **Event Viewer** logs on all domain controllers for any errors or warnings related to Active Directory or replication.

3. **Test Application Functionality**:
   - Verify that all AD-integrated applications and services are functioning correctly after the functional level raise.

4. **Update Documentation**:
   - Document the changes made, including the new functional levels and any issues encountered during the upgrade process.

### **Important Notes**

- **Reversibility**: Raising the functional level is a one-way operation. You cannot lower the functional level once it has been raised, so ensure you are fully prepared before proceeding.
- **Replication Latency**: After raising the functional levels, there may be some replication latency, so allow time for changes to propagate across all domain controllers in the forest.

By following these steps, you can successfully raise the domain and forest functional levels of your Active Directory environment to Windows Server 2016, enabling new features and capabilities provided by this functional level.


---

# Demoting the Domain Controller

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