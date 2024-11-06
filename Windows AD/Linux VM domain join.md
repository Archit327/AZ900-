
Joining a Linux VM to an Active Directory (AD) domain involves a few steps, including installing the necessary packages and configuring the Linux system to authenticate with the AD domain. Here’s a step-by-step guide:

### Prerequisites
- **Linux VM** with network connectivity to the AD Domain Controller.
- **Domain Controller (DC) IP address** and **Domain Name**.
- **AD Domain Admin** account credentials for joining the domain.
- **Network Configuration**: Ensure DNS is set to resolve the AD domain, usually by pointing to the AD DCs as primary DNS servers.

### Step-by-Step Guide

#### 1. **Update Packages**
   - Update your Linux system packages:
     ```bash
     sudo apt update
     sudo apt upgrade -y
     ```

Become super user
 
 sudo su - 
 
 now the $ will become # 
 
 $- normal user
 # - superuser
#### 2. **Install Required Packages**
   - For Debian-based systems (Ubuntu):
     ```bash
     sudo apt install realmd sssd adcli krb5-user samba-common-bin -y
     ```
  #### 3. **Configure DNS**
   - Ensure the DNS server is set to the IP address of the AD Domain Controller. For example:
     ```bash
     sudo vi /etc/resolv.conf
     ```

editor box is shown 
type "i" start insert
#### 4. **Discover the AD Domain**
   - Verify that the domain can be discovered by using the `realm` command:
     ```bash
     realm discover yourdomain.com
     ```
   - You should see domain information if the discovery is successful.

#### 5. **Join the AD Domain**
   - Join the domain using `realm`:
     ```bash
     sudo realm join --user=adminuser yourdomain.com
     ```
   - Replace `adminuser` with a domain admin username. You’ll be prompted to enter the password.

#### 6. **Configure SSSD for Authentication**
   - Open the SSSD configuration file:
     ```bash
     sudo nano /etc/sssd/sssd.conf
     ```
   - Ensure it contains the following configuration:
     ```ini
     [sssd]
     services = nss, pam
     config_file_version = 2
     domains = yourdomain.com

     [domain/yourdomain.com]
     id_provider = ad
     access_provider = ad
     ```

   - Set the correct permissions for the configuration file:
     ```bash
     sudo chmod 600 /etc/sssd/sssd.conf
     ```

#### 7. **Start and Enable SSSD Service**
   - Enable and start SSSD to handle authentication:
     ```bash
     sudo systemctl enable sssd
     sudo systemctl start sssd
     ```

#### 8. **Update PAM Configuration**
   - Enable home directory creation for AD users:
     ```bash
     sudo pam-auth-update
     ```
   - In the interface, select **Create home directory on login** and apply the changes.

#### 9. **Test AD Login**
   - You can test AD login by switching to an AD account:
     ```bash
     su - 'domainuser@yourdomain.com'
     ```
   - Or log in via SSH:
     ```bash
     ssh domainuser@yourdomain.com@<Linux_VM_IP>
     ```

After completing these steps, your Linux VM should be joined to the AD domain, and you’ll be able to authenticate using AD credentials. Let me know if you need any further clarification on these steps!



----


To confirm if the installation command `sudo apt install realmd sssd adcli krb5-user samba-common-bin -y` ran successfully, you can check in two main ways:

### 1. **Verify Command Output During Installation**
   - When you run the command, observe the terminal output. A successful installation typically ends with a message like:
     ```
     Processing triggers for man-db (version) ...
     ```
   - If there were any issues, they would appear as error messages, typically indicating missing packages or connection issues.

### 2. **Check Installed Packages Individually**
   - Confirm each package is installed by using the `dpkg -l` command:
     ```bash
     dpkg -l | grep -E 'realmd|sssd|adcli|krb5-user|samba-common-bin'
     ```
   - You should see entries like:
     ```
     ii  realmd           version        description
     ii  sssd             version        description
     ii  adcli            version        description
     ii  krb5-user        version        description
     ii  samba-common-bin version        description
     ```
   - The `ii` in the output signifies that the package is installed.

This will confirm whether each package was installed successfully. If any package is missing, rerun the install command specifically for the missing package.