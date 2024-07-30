

### Understanding External DNS Servers

### What are External DNS Servers?

External DNS servers are DNS servers that are located outside the organization's internal network and are typically managed by external entities such as ISPs, cloud service providers, or other third-party DNS service providers. These servers are responsible for resolving domain names for resources outside the internal network, such as websites, email servers, and other internet-based services.

### Importance of Knowing External DNS Servers

1. **Network Configuration**:
   - **DNS Resolution**: Ensures that devices within the network can resolve external domain names correctly.
   - **Redundancy**: Provides redundancy and reliability in DNS resolution by having multiple external DNS servers configured.

2. **Troubleshooting**:
   - **DNS Issues**: Identifying and resolving issues related to DNS resolution for external resources.
   - **Performance**: Ensuring optimal performance and response times for DNS queries.

3. **Security**:
   - **DNS Filtering**: Implementing security measures such as DNS filtering to block malicious domains.
   - **Monitoring**: Monitoring DNS traffic to detect and mitigate DNS-based attacks.

### Gathering Information About External DNS Servers

To identify the external DNS servers used by an organization, you can:

1. **Check Network Configuration**:
   - **Windows**:
     - Open Command Prompt and run `ipconfig /all`.
     - Look for the DNS Servers section under the network adapter details.
   - **Linux**:
     - Open Terminal and check the contents of the `/etc/resolv.conf` file.
     - Look for lines starting with `nameserver` followed by the IP address of the DNS server.

2. **Consult Network Documentation**:
   - Review network documentation or configuration files maintained by the IT department.
   - Look for information about DNS server configurations.

3. **Check DHCP Server Settings**:
   - DHCP servers often provide DNS server information to clients.
   - Check the DHCP server configuration to see which DNS servers are being assigned to network clients.

4. **DNS Server Configuration on Routers/Firewalls**:
   - Check the DNS server settings configured on network routers or firewalls.

### Example Response to Why This Information is Needed

If someone in the organization asks why it is important to know the external DNS servers, you could explain:

---

**"Understanding which external DNS servers are used by our organization is crucial for several reasons:

1. **Network Configuration**: Ensures that devices within our network can resolve external domain names correctly, providing reliable access to internet-based resources.
2. **Troubleshooting**: Helps in identifying and resolving DNS-related issues that affect access to external websites, email servers, and other services.
3. **Performance**: Ensures that we have optimal performance and response times for DNS queries, improving overall network efficiency.
4. **Security**: Allows us to implement security measures such as DNS filtering and monitoring to block malicious domains and detect DNS-based attacks."**

---

This explanation should help clarify the importance of knowing the external DNS servers used by the organization.

### Example External DNS Servers Information

Here is an example of how the information about external DNS servers might be documented:

```plaintext
External DNS Servers Configuration

1. **Primary External DNS Server**:
   - **Type**: Windows
   - **IP Address**: 8.8.8.8
   - **Hostname**: dns.google

2. **Secondary External DNS Server**:
   - **Type**: Windows
   - **IP Address**: 8.8.4.4
   - **Hostname**: dns.google

3. **Tertiary External DNS Server**:
   - **Type**: Linux
   - **IP Address**: 1.1.1.1
   - **Hostname**: one.one.one.one (Cloudflare)

4. **Quaternary External DNS Server**:
   - **Type**: Linux
   - **IP Address**: 1.0.0.1
   - **Hostname**: one.one.one.one (Cloudflare)
```

### Steps to Document External DNS Servers

1. **Identify DNS Servers**:
   - Use the methods mentioned above to identify the external DNS servers in use.
   
2. **Record Details**:
   - For each DNS server, record the following details:
     - **Type**: Whether the server is Windows or Linux-based.
     - **IP Address**: The IP address of the DNS server.
     - **Hostname**: The hostname of the DNS server (if available).

3. **Verify and Update Documentation**:
   - Verify the accuracy of the information with network administrators.
   - Update network documentation to include the details of the external DNS servers.

By following these steps, you can ensure that the external DNS servers used by your organization are properly documented and understood.