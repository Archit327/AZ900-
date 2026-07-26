
Complete Setup – Disk Buffer & Logrotate 
Overview
This document provides a comprehensive guide for setting up syslog-ng log collection with size-based logrotate functionality. The setup collects logs from multiple linux and windows machines and segregates them based on IP addresses OS types with automatic rotation when files reach 1GB.
Prerequisites
1. Network Configuration
Firewall Settings (collector-new)
# Allow syslog port 514 for both TCP and UDP
sudo ufw allow 514/tcp
sudo ufw allow 514/udp
sudo ufw reload

# Verify firewall status
sudo ufw status verbose
 
Cloud Security Groups
If using Azure/AWS/GCP, ensure security groups allow:
•	Inbound TCP port 514 from source IPs 
•	Inbound UDP port 514 from source IPs
2. Directory Structure Setup
Create Disk Buffer Directories
# Create disk buffer directories for syslog-ng
sudo mkdir -p /mnt/log-buffer/linux
sudo mkdir -p /mnt/log-buffer/windows
sudo mkdir -p /mnt/log-buffer/all-clients

# Set proper permissions
sudo chown root:root /mnt/log-buffer/*
sudo chmod 755 /mnt/log-buffer/*

# Verify directory creation
sudo ls -la /mnt/log-buffer/
 
Note- Locations can vary, kindly review the naming of the files and folders.

Create Log Rotation Directories

# Create directories for rotated logs
sudo mkdir -p /var/log/syslog-ng/rotated-linux
sudo mkdir -p /var/log/syslog-ng/rotated-windows
sudo mkdir -p /var/log/syslog-ng/rotated-all-clients

# Set proper permissions
sudo chown root:root /var/log/syslog-ng/rotated-*
sudo chmod 755 /var/log/syslog-ng/rotated-*

# Verify directory creation
sudo ls -la /var/log/syslog-ng/
 
Note- Locations can vary, kindly review the naming of the files and folders.
3. Software Installation
Syslog-ng Configuration
Add the syslog-ng repository and install it from the official source.
sudo apt install syslog-ng -y 
 
Complete Syslog-ng Configuration File with Disk Buffer
Location: /etc/syslog-ng/syslog-ng.conf
@version: 3.35 @include "scl.conf"

options {
    flush_lines(0);
    log_fifo_size(1000);
    chain_hostnames(no);
    use_dns(no);
    use_fqdn(no);
    dns_cache(no);
    keep_hostname(yes);
    owner("root");
    group("root");
    perm(0640);
    stats_freq(60);
    time_reopen(10);
};

# ────── Source: Remote logs ──────
source s_remote {
    tcp(ip(0.0.0.0) port(514));
    udp(ip(0.0.0.0) port(514));
};

# ────── Destinations ──────
destination d_linux {
    file("/var/log/syslog-ng/Linux.log"
         template("${ISODATE} ${HOST} ${MSG}\n")
         create-dirs(yes) owner("root") group("root") perm(0640)
         disk-buffer(
             reliable(yes)
             mem-buf-size(209715200) #200MB
             disk-buf-size(1099511627776) #1TB
             dir("/mnt/log-buffer/linux")
         )
    );
};

destination d_windows {
    file("/var/log/syslog-ng/Windows.log"
         template("${ISODATE} ${HOST} ${MSG}\n")
         create-dirs(yes) owner("root") group("root") perm(0640)
         disk-buffer(
             reliable(yes)
             mem-buf-size(209715200) #200MB
             disk-buf-size(1099511627776) #1TB
             dir("/mnt/log-buffer/windows")
         )
    );
};

destination d_all_clients {
    file("/var/log/syslog-ng/all-clients.log"
         template("${ISODATE} ${HOST} ${MSG}\n")
         create-dirs(yes) owner("root") group("root") perm(0640)
         disk-buffer(
             reliable(yes)
             mem-buf-size(209715200) #200MB
             disk-buf-size(1099511627776) #1TB
             dir("/mnt/log-buffer/all-clients")
         )
    );
};

# ────── Filters ──────
filter f_linux {
    match("PRIVATEIP=10\.0\.0\." value("MESSAGE"))
    or match("PRIVATEIP=172\.17\.0\.4" value("MESSAGE"));
};

filter f_windows {
    match("PRIVATEIP=10\.0\.0\.5" value("MESSAGE"));
};

filter f_other {
    not filter(f_linux) and not filter(f_windows);
};

# ────── Log Paths ──────
log {
    source(s_remote);
    filter(f_linux);
    destination(d_linux);
};

log {
    source(s_remote);
    filter(f_windows);
    destination(d_windows);
};

log {
    source(s_remote);
    filter(f_other);
    destination(d_all_clients);
};
 
 
Restart and Verify syslog-ng:
sudo systemctl restart syslog-ng 
sudo systemctl enable syslog-ng 
sudo systemctl status syslog-ng 

sudo syslog-ng --syntax-only
Configuration Explanation
Options Section
•	flush_lines(0): Immediately flush messages to disk (critical for logrotate)
•	time_reopen(10): Check for file changes every 10 seconds (enables rotation without SIGHUP)
•	use_dns(no): Disable DNS lookups for performance
•	stats_freq(60): Generate statistics every 60 seconds
Source Section
•	TCP/UDP 514: Standard syslog ports
Destinations Section
•	Disk Buffers: 200MB memory + 1TB disk buffer for reliability
•	File Permissions: 640 (owner read/write, group read)
•	Templates: Include timestamp, hostname, and message
Filters Section (Managed under Nxlog)
•	Linux Filter: Matches PRIVATEIP=10.0.0.x and 172.17.0.4
•	Windows Filter: Matches PRIVATEIP with range of Windows servers
•	Other Filter: Everything that doesn't match Linux or Windows like Firewall
Logrotate Configuration
Overview
Logrotate uses the copytruncate method to avoid file descriptor issues:
1.	Copy log content to rotated file
2.	Truncate original file to 0 bytes
3.	syslog-ng continues writing to same file descriptor
4.	No signals required (prevents memory leaks)
Linux Logs Configuration
Location: /etc/logrotate.d/syslog-ng-linux
/var/log/syslog-ng/Linux.log {
    size 1G
    rotate 15
    missingok
    nocompress
    notifempty
    copytruncate
    dateext
    dateformat -%Y%m%d-%H%M%S
    olddir /var/log/syslog-ng/rotated-linux
    sharedscripts
    postrotate
        [ -d /var/log/syslog-ng/rotated-linux ] || mkdir -p /var/log/syslog-ng/rotated-linux
    endscript
}
 


Windows Logs Configuration
Location: /etc/logrotate.d/syslog-ng-windows
/var/log/syslog-ng/Windows.log {
    size 1G
    rotate 15
    missingok
    nocompress
    notifempty
    copytruncate
    dateext
    dateformat -%Y%m%d-%H%M%S
    olddir /var/log/syslog-ng/rotated-windows
    sharedscripts
    postrotate
        [ -d /var/log/syslog-ng/rotated-windows ] || mkdir -p /var/log/syslog-ng/rotated-windows
    endscript
}
 


All-Clients Logs Configuration (Firewall Logs) 
Location: /etc/logrotate.d/syslog-ng-all-clients
/var/log/syslog-ng/all-clients.log {
    size 5G 
    rotate 20
    missingok
    nocompress
    notifempty
    copytruncate
    dateext
    dateformat -%Y%m%d-%H%M%S
    olddir /var/log/syslog-ng/rotated-all-clients
    sharedscripts
    postrotate
        [ -d /var/log/syslog-ng/rotated-all-clients ] || mkdir -p /var/log/syslog-ng/rotated-all-clients
    endscript
}
 
Logrotate Directive Explanation
Directive	Purpose
size 50M	Rotate when file reaches 50 megabytes
rotate 10	Keep 10 rotated copies before deletion
missingok	Don't error if log file is missing
notifempty	Don't rotate empty files
copytruncate	Copy and truncate instead of moving file
dateext	Use date extension instead of numbers
dateformat -%Y%m%d-%H%M%S	Custom date format (YYYYMMDD-HHMMSS)
olddir	Directory to move rotated files
sharedscripts	Run postrotate script only once

Automation Setup
Combined Execution Script
Location: /usr/local/bin/logrotate-syslog-ng.sh
#!/bin/bash
# Combined script to run all syslog-ng logrotate configurations
/usr/sbin/logrotate /etc/logrotate.d/syslog-ng-linux
/usr/sbin/logrotate /etc/logrotate.d/syslog-ng-windows  
/usr/sbin/logrotate /etc/logrotate.d/syslog-ng-all-clients

# Optional: Log the execution
echo "$(date): Logrotate executed for syslog-ng logs" >> /var/log/logrotate-syslog-ng.log
 

Make script executable:
sudo chmod +x /usr/local/bin/logrotate-syslog-ng.sh
 
Cron Job Configuration
Size-based logrotate requires frequent execution to check file sizes.
# Add cron job to run every 1 minutes
echo "*/1 * * * * /usr/local/bin/logrotate-syslog-ng.sh >/dev/null 2>&1" | sudo crontab -

# Verify cron job is set
sudo crontab -
 
Note- The cron job frequency can be varied for different sizes of files
#different cron job frequency for different files.
sudo crontab -r 

# Add new cron jobs with different frequencies
(echo "*/1 * * * * /usr/local/bin/logrotate-frequent.sh >/dev/null 2>&1"; echo "*/10 * * * * /usr/local/bin/logrotate-all-clients.sh >/dev/null 2>&1") | sudo crontab -
 

How Logrotate is Triggered
Workflow Overview
1.	Cron Execution: Every 1 minutes, cron runs the logrotate script
2.	Size Check: Logrotate checks if any files exceed 1GB
3.	Rotation Trigger: If threshold exceeded, rotation begins
4.	Copy Process: Current log content copied to timestamped file
5.	Truncate: Original log file truncated to 0 bytes
6.	Directory Move: Rotated file moved to appropriate rotated-* directory
7.	Continuation: syslog-ng continues writing to same file descriptor
Timeline Example
10:00 - all-clients.log (995 MB) - No action
10:01 - all-clients.log (1GB) - Rotation triggered
       ↓
       Copy: all-clients.log → rotated-all-clients/all-clients.log-20250828-100100
       ↓
       Truncate: all-clients.log becomes 0 bytes
       ↓
       syslog-ng continues writing to all-clients.log
10:02 - all-clients.log (3MB, growing) - No action

File Structure After Rotation
/var/log/syslog-ng/
├── Linux.log                    # Active log (0-1024MB)
├── Windows.log                  # Active log (0-1024MB)  
├── all-clients.log              # Active log (0-5GB)
├── rotated-linux/
│   ├── Linux.log-20250828-105501
│   └── Linux.log-20250828-110002  #whenever the file reaches to 1G
├── rotated-windows/
│   └── Windows.log-20250828-105501
└── rotated-all-clients/
    ├── all-clients.log-20250828-105501
    └── all-clients.log-20250828-110002

Testing and Verification
Initial Testing
# 1. Check syslog-ng status
sudo systemctl status syslog-ng

# 2. Verify listening ports
sudo netstat -tulpn | grep :514

# 3. Test configuration syntax
sudo syslog-ng --syntax-only

# 4. Test logrotate configurations
sudo logrotate -d /etc/logrotate.d/syslog-ng-linux
sudo logrotate -d /etc/logrotate.d/syslog-ng-windows  
sudo logrotate -d /etc/logrotate.d/syslog-ng-all-clients
 
Connectivity Testing
# From ubuntu01, test connection
telnet <collector-new-ip> 514
echo "Test message from ubuntu01" | nc <collector-new-ip> 514

# On collector-new, check for log files
ls -la /var/log/syslog-ng/
tail -f /var/log/syslog-ng/all-clients.log
 

Force Rotation Testing
# Force rotation to test mechanism
sudo logrotate -f /etc/logrotate.d/syslog-ng-all-clients

# Check results
ls -la /var/log/syslog-ng/
ls -la /var/log/syslog-ng/rotated-all-clients/
Monitoring and Maintenance
Check Disk Buffers
# List buffer files
ls -la /mnt/log-buffer/*/

# Check buffer content
sudo strings /mnt/log-buffer/all-clients/*.rqf | wc -l
sudo strings /mnt/log-buffer/all-clients/*.rqf | head -10

# Monitor buffer statistics
sudo syslog-ng-ctl stats | grep buffer
 
Monitor Log Rotation
# Check cron execution
sudo tail -f /var/log/cron | grep logrotate

# Check logrotate activity log
sudo tail -f /var/log/logrotate-syslog-ng.log

# Monitor file sizes
watch 'ls -lh /var/log/syslog-ng/*.log' 
Troubleshooting
# Check syslog-ng logs
sudo journalctl -xeu syslog-ng.service

# Run syslog-ng in debug mode
sudo systemctl stop syslog-ng
sudo syslog-ng -F -d
# Press Ctrl+C to stop, then restart service

# Check disk space
df -h /var/log
df -h /mnt/log-buffer

# Verify cron job
sudo crontab -l
sudo systemctl status cron
 
Performance Considerations
Disk Buffer Sizing
•	Memory Buffer: 200MB (configured) - holds recent messages
•	Disk Buffer: 1TB (configured) - overflow storage
•	Total: ~1TB per destination for buffering
Log Rotation Frequency
•	Check Interval: Every 1 minutes via cron (Can be varied according to the log ingestion)
•	Rotation Threshold: 1GB per file, 5GB for Firewall logs file
•	Retention: 15 rotated files per log type, 20 - FW logs file
Security Considerations
File Permissions
•	Log Files: 640 (owner: read/write, group: read, others: none)
•	Directories: 755 (owner: full, group/others: read/execute)
•	Owner/Group: root:root
Network Security
•	Firewall: Only allow port 514 from known sources
•	Cloud Security: Restrict access to specific IP ranges
•	No Encryption: Consider TLS if transmitting sensitive data
Access Control
•	Log Access: Only root can read log files
•	Buffer Access: Only root can access disk buffers
•	Configuration: Only root can modify syslog-ng/logrotate configs

