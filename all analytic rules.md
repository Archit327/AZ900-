
PaloAlto Threat Analytic rules

1.	Palo Alto – Repeated Insecure or Default Credential Authentication Attempts

Description –
This analytic rule detects repeated authentication attempts using insecure, default, or anonymous credentials identified by Palo Alto threat signatures categorized as a vulnerability (for example, manufacturer default credentials or anonymous password usage). An alert is generated when the same source IP triggers ten or more such events within the rule query period and the firewall action is recorded as allow or alert. These events typically indicate automated credential probing or scanning activity against exposed services (such as FTP). This rule does not confirm successful authentication or account compromise but highlights repeated weak credential attempts that may indicate reconnaissance or brute-force preparation activity.
Severity - Medium
MITRE ATT&CK
•	T1110 – Brute Force
•	T1078 – Valid Accounts
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")
| where DeviceEventCategory == "vulnerability"
| extend ThreatCategory = extract(@"PanOSThreatCategory=([^;]+)", 1, AdditionalExtensions)
| where ThreatCategory == "insecure-credentials"
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    DestIPCount = dcount(DestinationIP)
by SourceIP, DeviceEventClassID, DeviceName
| where EventCount >= 10

Entity Mapping
•	IP → SourceIP
•	Account → SourceUserName
•	Host → DeviceName
________________________________________
 2.	Repeated Suspicious DNS Communication

Name - Palo Alto – PAN Repeated Suspicious DNS Communication
Description – 
This analytic rule detects repeated DNS-based threat activity identified by Palo Alto Networks threat signatures.
An alert is generated when:
•	A Palo Alto THREAT log is recorded
•	The traffic action is allow or alert (not blocked)
•	The protocol is DNS
•	The event category is spyware, malware, or command-and-control
•	The same Source IP and threat signature generate three or more events within the analytic rule lookup period
Severity - High
MITRE ATT&CK
•	T1048 – Exfiltration Over Alternative Protocol
•	T1071.004 – DNS
•	T1071.004 – Application Layer Protocol: DNS
•	T1048 – Exfiltration Over Alternative Protocol
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity contains "Threat"
| where SimplifiedDeviceAction in ("allow","alert")
| where ApplicationProtocol contains "dns"
| where DeviceEventCategory in ("spyware","malware","command-and-control")
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    DistinctDomains = dcount(DestinationDnsDomain),
    TotalSentBytes = sum(SentBytes)
by SourceIP, DeviceEventClassID, DeviceName
| where EventCount >= 3
Entity Mapping
•	IP → SourceIP
•	Host → DeviceName
________________________________________
 3.	Active Directory Attack (DCSync)

Name - PAN – Suspicious Active Directory Replication Activity (DCSync)
Description –
This analytic rule detects repeated Active Directory replication (DCSync)–related threat signatures identified by Palo Alto Networks. An alert is generated when allowed or alerted (not blocked) traffic contains DCSync-related signatures and the same source IP targets the same destination IP two or more times within the rule lookup period. DCSync behavior may indicate attempts to replicate domain credentials and is commonly associated with credential dumping techniques. This rule highlights repeated suspicious replication attempts but does not confirm successful credential extraction or domain compromise.
Severity - High
MITRE ATT&CK
•	T1003.006 – DCSync
•	T1003 – OS Credential Dumping
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")   // ignore blocked
| where DeviceEventClassID has_cs "DCSync"
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DestinationIP, DeviceEventClassID, DeviceName, Activity, SimplifiedDeviceAction
| where EventCount >= 3
 
Entity Mapping
•	IP → SourceIP
•	IP → DestinationIP
________________________________________
 4.	Reconnaissance Activity (Nmap / Scanners)

Name - PAN – Suspicious Network Service Discovery Activity
Description –
This analytic rule detects suspicious network service discovery activity identified by Palo Alto threat signatures. An alert is generated when allowed or alerted (not blocked) traffic indicates scanning-related behavior and a single source IP generates five or more reconnaissance-related events targeting multiple destination IPs within the rule lookup period. Such activity may indicate network discovery, service enumeration, or pre-attack reconnaissance. This rule highlights potential scanning behavior but does not confirm malicious exploitation.
Severity - Medium
MITRE ATT&CK
•	T1046 – Network Service Discovery
•	T1595 – Active Scanning (if external scanning)
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")
| where DeviceEventClassID has "Nmap"
| summarize
    EventCount = count(),
    Destinations = make_set(DestinationIP, 20)
by SourceIP, DeviceEventClassID, DeviceName
| where EventCount >= 5

Entity Mapping
•	IP → SourceIP
________________________________________
 5.	Brute Force Attempts

Name - PAN – Suspicious Authentication Brute-Force Activity
Description –
This analytic rule identifies suspicious authentication brute-force activity based on Palo Alto threat signatures. An alert is generated when allowed or alerted (not blocked) traffic contains brute-force–related signatures and the same source IP targets the same destination IP three or more times within the analytic rule lookup period. Such behavior may indicate password-guessing attempts, credential stuffing, or repeated authentication probing against a target system. This rule highlights potentially unauthorized authentication activity but does not confirm successful account compromise.
Severity - High
MITRE ATT&CK
•	T1110 – Brute Force
•	T1110.001 – Password Guessing
•	T1110.003 – Credential Stuffing
KQL
CommonSecurityLog
| where Activity contains "threat"
| where SimplifiedDeviceAction in ("allow","alert")
| where DeviceEventClassID contains "brute"
| where DestinationPort in (21,22,23,25,110,143,389,445,3389,636)
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DestinationIP, DeviceEventClassID
| where EventCount >= 3
Entity Mapping
•	IP → SourceIP
•	IP → DestinationIP
________________________________________
 6.	Exploit Attempt

Name - Palo Alto – Exploit Attempt Detected
Description –
This rule detects exploit attempts identified by Palo Alto threat signatures, which may indicate attempts to exploit software vulnerabilities or deliver malicious payloads.
An alert is generated when:
•	A Palo Alto THREAT log indicates exploit-related activity, and
•	The traffic was allowed or alert (not blocked), and
•	The same Source IP targeting the same Destination IP generates 2 or more exploit-related events within the analytic rule lookup period (for example, within the last 1 hour, depending on the rule schedule).
The rule aggregates events to reduce noise and highlights repeated exploitation attempts against a target, which may indicate active vulnerability exploitation, malicious document delivery, or exploit kit activity.
Severity - High
MITRE ATT&CK
•	T1203 – Exploitation for Client Execution
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")
| where DeviceEventClassID has "Exploit"
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DestinationIP, DeviceEventClassID
| where EventCount >= 2
 

Entity Mapping
•	IP → SourceIP
•	IP → DestinationIP
________________________________________
 7.	Lateral Movement (Remote Execution)

Name - Palo Alto – Suspicious Remote Service Activity
Description –
This analytic rule detects remote execution–related threat signatures identified by Palo Alto Networks. An alert is generated when allowed or alerted (not blocked) traffic indicates remote execution–related activity and the same source IP targets the same destination IP two or more times within the analytic rule lookup period. Such behavior may indicate lateral movement, remote administration tool misuse, or post-exploitation activity. This rule highlights suspicious remote execution activity but does not confirm successful command execution or system compromise.
Severity - High
MITRE ATT&CK
•	T1021 – Remote Services
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity contains "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")   // Ignore blocked traffic
| where DeviceEventClassID has_cs "Remote Execution"
// | where ThreatSeverity >= 3
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DestinationIP, DeviceEventClassID, DeviceName
| where EventCount >= 3 

Entity Mapping
•	IP → SourceIP
•	IP → DestinationIP
________________________________________
 8.	Web Attacks (SQL Injection)

Name - Palo Alto – Web Attack Detected
Description –
This rule detects SQL injection or database attack attempts identified by Palo Alto threat signatures, which may indicate attempts to exploit vulnerabilities in web applications or backend databases.
An alert is generated when:
•	A Palo Alto THREAT log indicates SQL injection or SQL-related attack activity, and
•	The traffic was allowed or alert (not blocked), and
•	At least one SQL-related threat event is observed within the analytic rule lookup period (for example, within the last 1 hour, depending on the rule schedule).
Because SQL injection alerts are typically high confidence and lower volume, aggregation or thresholds are not required in this rule.
The alert highlights potential attempts to manipulate database queries, bypass authentication, or extract sensitive data from web applications.
Severity - High
MITRE ATT&CK
•	T1190 – Exploit Public-Facing Application
KQL
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks"
| where Activity contains "THREAT"
| where SimplifiedDeviceAction in ("allow","alert")
| where DeviceEventClassID has "SQL"
| where LogSeverity contains "3" or LogSeverity contains "4" or LogSeverity contains "5"

Entity Mapping
•	IP → SourceIP
•	IP → DestinationIP

 9.	Excessive Firewall Denies from Remote Host

Description: 
This KQL query detects potential port scanning or excessive connection attempts from a single remote host by analyzing firewall deny events in the CommonSecurityLog. It filters logs from the last 5 minutes where the firewall action is deny, then aggregates events by SourceIP to count the number of denied connections and the number of unique destination ports targeted. The query also collects contextual information such as targeted destination IPs and hostnames, protocols used, and firewall device details to assist investigation. An alert condition is triggered when a single source generates more than 100 denied attempts and targets more than 10 unique ports, which may indicate port scanning, reconnaissance activity, or aggressive probing of services.
Query:
CommonSecurityLog
| where TimeGenerated >= ago(5m)
| where DeviceAction == "deny" // --- Focus on REMOTE (external) sources ---
| where not(ipv4_is_private(SourceIP))
// --- Optional: ensure target is internal (recommended) ---
| where ipv4_is_private(DestinationIP)
// --- Aggregation ---
| summarize
    DenyCount = count(),
    UniquePorts = dcount(DestinationPort),
    Ports = strcat_array(make_set(DestinationPort, 10), ", "),
    DestinationIPs = strcat_array(make_set(DestinationIP, 10), ", "),
    DestinationHosts = strcat_array(make_set(DestinationHostName, 10), ", "),
    Protocols = strcat_array(make_set(Protocol, 10), ", "),
    DeviceProducts = strcat_array(make_set(DeviceProduct, 5), ", "),
    DeviceVendors = strcat_array(make_set(DeviceVendor, 5), ", ")
    by SourceIP
// --- Thresholds ---
| where DenyCount > 10
| where UniquePorts > 10
| order by DenyCount desc
 

________________________________________
 10.	Potential Botnet Events from Firewall Threat Logs

Description:
This use case detects internal hosts that repeatedly trigger botnet-related threat alerts in firewall logs ingested into Microsoft Sentinel. The query analyzes the CommonSecurityLog table for events categorized as Botnet under IndicatorThreatType where the firewall generated an alert or allowed the connection. By aggregating events by SourceIP over a 24-hour period and applying a threshold, the detection identifies hosts that repeatedly communicate with suspected botnet infrastructure. The output includes contextual information such as first and last observed activity, destination IPs, ports, and protocols, enabling security analysts to quickly identify potentially compromised systems and investigate possible command-and-control communication.
CommonSecurityLog
| where TimeGenerated >= ago(24h)
| where IndicatorThreatType contains "Botnet"
| where DeviceAction in ("alert", "allow", "allowed")
| summarize
    BotnetEventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    DestinationIPs = strcat_array(make_set(DestinationIP, 10),", "),
    DestinationPorts = strcat_array(make_set(DestinationPort, 10),", "),
    Protocols = strcat_array(make_set(Protocol, 10), ", "),
by SourceIP, IndicatorThreatType
| where BotnetEventCount >= 5
| sort by BotnetEventCount desc

Results:
SourceIP	IndicatorThreatType	BotnetEventCount	FirstSeen	LastSeen	DestinationIPs	DestinationPorts	Protocols
82.165.66.87	Botnet	46	Mar 10, 2026 10:09:06 AM	Mar 10, 2026 10:09:49 AM	["61.220.240.3"]	[443]	["tcp"]
89.248.168.239	Botnet	8	Mar 9, 2026 11:11:12 PM	Mar 10, 2026 2:55:16 AM	["172.16.1.10","10.86.100.34"]	[443]	["tcp"]
185.12.59.117	Botnet	6	Mar 9, 2026 11:02:23 AM	Mar 9, 2026 3:24:40 PM	["10.86.100.34"]	[443]	["tcp"]
185.224.128.251	Botnet	6	Mar 9, 2026 11:52:14 AM	Mar 10, 2026 1:31:56 AM	["61.220.240.3"]	[443]	["tcp"]
79.124.40.174	Botnet	6	Mar 10, 2026 8:40:14 AM	Mar 10, 2026 8:54:46 AM	["61.220.240.3","10.86.100.34"]	[443]	["tcp"]
45.205.1.8	Botnet	6	Mar 9, 2026 11:54:09 AM	Mar 9, 2026 3:08:22 PM	["61.220.240.3"]	[443]	["tcp"]
176.65.149.253	Botnet	5	Mar 9, 2026 1:51:49 PM	Mar 10, 2026 2:31:23 AM	["10.86.100.34","61.220.240.3","172.16.1.10"]	[443]	["tcp"]
185.12.59.118	Botnet	5	Mar 9, 2026 1:01:54 PM	Mar 9, 2026 2:22:47 PM	["61.220.240.3","10.86.100.34"]	[443]	["tcp"]

FYI, the below query provides the following information in the workspace. The above query works on thresholds, we can trigger an alert on all allowed Botnet logs if needed.

CommonSecurityLog
| where Activity contains "threat"
| where DeviceAction in ("alert","allow")
| summarize count = count() by IndicatorThreatType

Results based on 24 hrs data.
IndicatorThreatType	count
	30200717
Botnet	125
MaliciousUrl	21
C2	319
Proxy	1

________________________________________
11.	Local TCP Scanner Detected containing Session Denied

Description:
This analytic rule detects potential internal TCP scanning activity originating from a host within the internal network. It identifies situations where a single source IP attempts connections to more than 59 unique internal destination hosts within a 10-minute window over TCP while the connections are denied by the firewall. The rule excludes common service ports and known vulnerability scanning systems to reduce false positives. Such behavior may indicate reconnaissance activity, lateral movement preparation, or unauthorized network scanning performed by a compromised host or malicious insider.

Query:
let ExcludedPorts = dynamic([
    21, 22, 25, 53, 67, 68, 69, 80, 110, 119, 123, 135, 137, 138, 139,
    143, 161, 162, 179, 389, 443, 445, 465, 500, 514, 515, 520,
    587, 636, 989, 990, 993, 995, 1433, 1434, 1521, 2049,
    2082, 2083, 2086, 2087, 2181, 2483, 2484, 3000,
    3306, 3389, 3690, 4444, 5432, 5601, 5631, 5666,
    5800, 5900, 5985, 5986, 6379, 6667, 7001, 7002,
    8000, 8008, 8009, 8080, 8081, 8088, 8090, 8443, 8888, 9000
    ]);
let TenableScanners =
    _GetWatchlist('Tenable_scanners')
    | project ScannerIP = SearchKey;
CommonSecurityLog
| where TimeGenerated >= ago(12h)
| where DeviceAction == "deny"
| where Protocol =~ "TCP"
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
| where DestinationPort !in (ExcludedPorts)
| where DestinationTranslatedPort !in (ExcludedPorts)
| where DestinationPort !in (7680, 2077)
| where SourcePort !in (7680, 2077)
| where SourceIP !in (TenableScanners)
| summarize
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated),
    EventCount = count(),
    UniqueDestHosts = dcount(DestinationIP),
    DestinationIPs = make_set(DestinationIP, 100),
    DestinationPorts = make_set(DestinationPort, 50),
    DeviceActions = make_set(DeviceAction, 10),
    Protocols = make_set(Protocol, 10),
    Devices = make_set(DeviceVendor, 10)
    by SourceIP, bin(TimeGenerated, 10m)
| where UniqueDestHosts > 59
| project
    TimeGenerated,
    SourceIP,
    EventCount,
    UniqueDestHosts,
    DestinationIPs,
    DestinationPorts,
    DeviceActions,
    Protocols,
    Devices,
    StartTime,
    EndTime
| order by UniqueDestHosts desc

Mitre mipping
Tactic:
•	Reconnaissance
Techniques:
•	Active Scanning - T1595
•	Network Service Discovery – T1046
________________________________________
12.	Local UDP Scanner Detected containing Unknown Application

Description:
This analytic rule detects potential internal UDP reconnaissance activity where a single internal source host communicates with a large number of internal destination hosts using unknown or unidentified applications within a short time window. The query analyzes firewall logs and identifies instances where a source IP generates more than five UDP events to over 59 unique internal destination IP addresses within 10 minutes. It focuses on traffic classified as unknown-udp, insufficient-data, incomplete, or not-applicable, which may indicate scanning tools or attempts to evade application identification. Known vulnerability scanners are excluded using a watchlist to reduce false positives. Such behavior can indicate network discovery or reconnaissance activity inside the environment, which may precede lateral movement or exploitation attempts.

Query:
let UnknownApps = dynamic([
    "unknown-udp",
    "insufficient-data",
    "incomplete",
    "not-applicable"
    ]);
let TenableScanners =
    _GetWatchlist('Tenable_scanners')
    | project ScannerIP = tostring(SearchKey);
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where Protocol contains "UDP"
| where ApplicationProtocol in (UnknownApps) or isempty(ApplicationProtocol)
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
| where SourceIP !in (TenableScanners)
| summarize
    EventCount = count(),
    UniqueDestIPs = dcount(DestinationIP),
    DestIPs = make_set(DestinationIP, 100),
    PortsTargeted = make_set(DestinationPort, 50),
    Applications = make_set(ApplicationProtocol, 10),
    Actions = make_set(DeviceAction, 10),
    Vendors = make_set(DeviceVendor, 5),
    Products = make_set(DeviceProduct, 5),
    Activities = make_set(Activity, 10),
    Protocols = make_set(Protocol, 5)
    by SourceIP, bin(TimeGenerated, 10m)
| where EventCount > 5
| where UniqueDestIPs > 59
| project
    TimeGenerated,
    SourceIP = tostring(SourceIP),
    EventCount = tostring(EventCount),
    UniqueDestIPs = tostring(UniqueDestIPs),
    Applications = strcat_array(Applications, ", "),
    PortsTargeted = strcat_array(PortsTargeted, ", "),
    DestIPs = strcat_array(DestIPs, ", "),
    Actions = strcat_array(Actions, ", "),
    Vendors = strcat_array(Vendors, ", "),
    Products = strcat_array(Products, ", "),
    Activities = strcat_array(Activities, ", "),
    Protocols = strcat_array(Protocols, ", ")
 

MITRE ATT&CK Mapping
Tactic:
•	Discovery
Techniques:
•	T1046 – Network Service Discovery
Adversaries may scan the network to identify active hosts and services using UDP probes.
•	T1018 – Remote System Discovery
The behavior of contacting many internal systems can indicate attempts to enumerate hosts within the network.
________________________________________
13.	Local TCP Scanner Detected containing Unknown Application

Description:
This analytic rule detects potential internal TCP reconnaissance activity by identifying a single internal source host initiating TCP connections to a large number of internal destination hosts within a short time window using unknown or unidentified applications. The rule analyzes firewall logs and flags cases where a source IP generates more than five TCP events targeting over 59 unique internal destination IP addresses within 10 minutes. Traffic associated with vulnerability scanners is excluded using a watchlist, and specific ports such as 7680 (Windows Update Delivery Optimization) and 2077 are excluded to reduce false positives. Such activity may indicate network scanning or host discovery attempts within the environment, potentially preceding lateral movement.




Query:
let UnknownApps = dynamic([
    "unknown-tcp",
    "insufficient-data",
    "incomplete",
    "not-applicable"
    ]);
let TenableScanners =
    _GetWatchlist('Tenable_scanners')
    | project ScannerIP = tostring(SearchKey);
let ExcludedPorts = dynamic([7680, 2077]);
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where Protocol contains "TCP"
| where ApplicationProtocol in (UnknownApps) or isempty(ApplicationProtocol)
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
| where SourceIP !in (TenableScanners)
| where DestinationPort !in (ExcludedPorts)
| summarize
    EventCount = count(),
    UniqueDestIPs = dcount(DestinationIP),
    DestIPs = make_set(DestinationIP, 100),
    PortsTargeted = make_set(DestinationPort, 50),
    Applications = make_set(ApplicationProtocol, 10),
    Actions = make_set(DeviceAction, 10),
    Vendors = make_set(DeviceVendor, 5),
    Products = make_set(DeviceProduct, 5),
    Activities = make_set(Activity, 10),
    Protocols = make_set(Protocol, 5)
    by SourceIP, bin(TimeGenerated, 10m)
| where EventCount > 5
| where UniqueDestIPs > 59
| project
    TimeGenerated,
    SourceIP = tostring(SourceIP),
    EventCount = tostring(EventCount),
    UniqueDestIPs = tostring(UniqueDestIPs),
    Applications = strcat_array(Applications, ", "),
    PortsTargeted = strcat_array(PortsTargeted, ", "),
    DestIPs = strcat_array(DestIPs, ", "),
    Actions = strcat_array(Actions, ", "),
    Vendors = strcat_array(Vendors, ", "),
    Products = strcat_array(Products, ", "),
    Activities = strcat_array(Activities, ", "),
    Protocols = strcat_array(Protocols, ", ")

Mitre mappings
Tactic
•	Discovery
Techniques
•	T1046 – Network Service Discovery
Adversaries may scan networks using TCP probes to identify active systems and services.
•	T1018 – Remote System Discovery
Contacting many internal hosts can indicate enumeration of systems within the network.
 14.	Potential RDP Lateral Movement Across Multiple Hosts

Description
This analytic rule detects a potential lateral movement attempt using Remote Desktop Protocol (RDP). It identifies a source host establishing successful RDP connections to many internal systems within a short time window. Such behaviour may indicate automated propagation, credential abuse, or an attacker moving laterally across the network after initial compromise.
Detection Logic Summary
The rule triggers when:
•	RDP traffic (port 3389) is allowed
•	A single source IP connects to more than 50 hosts
•	More than 50 RDP events occur within 10 minutes
This behaviour can indicate:
•	Compromised admin credentials
•	Automated lateral movement scripts
•	Malware or ransomware propagation
Mitre mapping
•	Lateral Movement
o	Remote Services: Remote Desktop Protocol - T1021.001
•	Discovery	
o	Network Service Discovery - T1046

Query:
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where DestinationPort == 3389
| where DeviceAction contains "allow"
| summarize
    HostsContacted = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 50),
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, Computer
| where HostsContacted > 50 and EventCount > 50
| project
    SourceIP,
    Computer,
    HostsContacted,
    EventCount,
    FirstSeen,
    LastSeen,
    Destinations
| order by HostsContacted desc
 
 15.	Potential SMB Lateral Movement Across Multiple Hosts

Description:
This analytic rule detects a potential lateral movement attempt using the Server Message Block (SMB) protocol. It identifies a source host establishing successful SMB connections to multiple internal systems within a short time window. Such behavior may indicate automated propagation, credential abuse, malware activity, or an attacker moving laterally across the network after an initial compromise.
The rule monitors firewall traffic logs and triggers when a single source IP communicates with more than ten unique destination hosts using SMB within a 10-minute period. Rapid SMB connections to multiple hosts are uncommon in normal user activity and may indicate suspicious network scanning or lateral movement activity.
MITRE ATT&CK Mapping
•	Tactic: Lateral Movement
o	Technique: Remote Services – SMB/Windows Admin Shares
o	Technique ID: T1021.002
This activity may also be associated with Discovery behavior where attackers scan internal systems to identify accessible services.
•	Tactic: Discovery
o	Technique: Network Service Discovery
o	Technique ID: T1046
Query:
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where ApplicationProtocol contains "smb"
| where DeviceAction contains "allow"
| summarize
    HostsContacted = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 50),
    Applications = make_set(ApplicationProtocol, 10),
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, SourceUserName, Computer, DeviceName
| where HostsContacted > 10
| extend
    Applications_str = strcat_array(Applications, ", "),
    Destinations_str = strcat_array(Destinations, ", ")
| project
    SourceIP,
    SourceUserName,
    Computer,
    DeviceName,
    Applications_str,
    Destinations_str
| order by SourceIP
 16.	Remote Desktop Access from Internet to Internal Host

Description – 
This analytic rule detects when an external public IP address initiates Remote Desktop Protocol (RDP) connections to an internal private host through the firewall. Such activity may indicate exposed RDP services, internet scanning, brute-force attempts, or unauthorized remote access attempts targeting internal systems. Monitoring this behavior helps identify potential initial access attempts by attackers exploiting externally accessible RDP services.
MITRE ATT&CK Mapping
Tactic
•	Initial Access
Techniques
•	T1133 – External Remote Services
•	T1021.001 – Remote Desktop Protocol
Query –
CommonSecurityLog
| where TimeGenerated >= ago(1h)
| where DestinationPort == 3389
| where DeviceAction contains "allow"
| where ipv4_is_private(SourceIP) == false
| where ipv4_is_private(DestinationIP) == true
| summarize 
    EventCount = count(),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated),
    DeviceName = any(DeviceName),
    DeviceVendor = any(DeviceVendor),
    DeviceProduct = any(DeviceProduct),
    ApplicationProtocol = any(ApplicationProtocol),
    DestinationHostName = any(DestinationHostName),
    DestinationDnsDomain = any(DestinationDnsDomain),
    CommunicationDirection = any(CommunicationDirection),
    DeviceAction = any(DeviceAction),
    SourcePorts = make_set(SourcePort),
    DestinationPorts = make_set(DestinationPort)
by SourceIP, DestinationIP
| project 
    StartTime,
    EndTime,
    EventCount,
    DeviceName,
    SourceIP,
    DestinationIP,
    DestinationPorts = strcat_array(DestinationPorts, ","),
    SourcePorts = strcat_array(SourcePorts, ","),
    DestinationHostName,
    DestinationDnsDomain,
    DeviceVendor,
    DeviceProduct,
    ApplicationProtocol,
    DeviceAction
| order by EventCount desc
 17.	Distributed Denial-of-Service (DDoS) attack

Description:
This analytic rule detects a potential Distributed Denial-of-Service (DDoS) attack targeting a single host by analyzing TCP traffic in firewall logs (CommonSecurityLog).
The rule identifies situations where many unique external source IP addresses communicate with the same destination IP and destination port within a 5-minute window.
If the number of unique external source hosts exceeds the defined threshold (e.g., 100 or more), it may indicate a coordinated volumetric attack attempting to overwhelm the target system or service.
This detection helps identify:
•	Distributed denial-of-service attempts
•	Botnet-driven traffic floods
•	Service exhaustion attempts on exposed services
The rule also enriches the alert with contextual information such as:
•	Source IP addresses involved in the activity
•	Firewall device names generating the logs
•	Device actions (allow/deny)
•	Start and end time of the activity window
Security analysts can use this information to quickly investigate the attack scope and identify the targeted system.
MITRE ATT&CK Mapping
•	Tactic: Impact
•	Technique: Network Denial of Service
•	Technique ID: T1498
Query:
CommonSecurityLog
| where TimeGenerated >= ago(5m)
| where Protocol =~ "TCP"
| where ipv4_is_private(SourceIP) == false
| where isnotempty(DestinationIP)
| where isnotempty(DestinationPort)
| summarize
    UniqueSourceHosts = dcount(SourceIP),
    TotalFlows = count(),
    SourceIPs = strcat_array(make_set(SourceIP, 100), ","),
    DeviceNames = strcat_array(make_set(DeviceName, 10), ","),
    DeviceActions = strcat_array(make_set(DeviceAction, 10), ","),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated),
    DeviceVendor = any(DeviceVendor),
    DeviceProduct = any(DeviceProduct)
by DestinationIP, DestinationPort
| where UniqueSourceHosts > 100
 18.	Local Windows Server Port Scanning Detected (Internal Reconnaissance)

Description:
This detection identifies potential internal reconnaissance activity where a single internal source IP attempts connections to Windows server service ports (such as SMB, RPC, or RDP) across many internal hosts within a short time window.
The rule analyses firewall or network device logs to detect a source communicating with more than 200 unique internal destination IP addresses within 20 minutes on common Windows ports. Known vulnerability scanners are excluded using a watchlist to reduce false positives.
Such activity may indicate network scanning, lateral movement preparation, or reconnaissance by an attacker or compromised host.
MITRE ATT&CK Mapping
Tactic
•	Reconnaissance
•	Discovery
Techniques
•	T1595 – Active Scanning
•	T1046 – Network Service Discovery
Query
let ScannerIPs = (
    _GetWatchlist('Tenable_scanners')
    | project SearchKey
    );
let WindowsPorts = dynamic([135, 139, 445, 3389, 5985, 5986]);
CommonSecurityLog
| where TimeGenerated >= ago(20m)
| where ipv4_is_private(SourceIP) == true
| where ipv4_is_private(DestinationIP) == true
| where SourceIP !in (ScannerIPs)
| where DestinationPort in (WindowsPorts)
| summarize
    EventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    DestinationIPs = strcat_array(make_set(DestinationIP, 200), ", "),
    DestinationPorts = strcat_array(make_set(DestinationPort, 10), ", "),
    DeviceActions = strcat_array(make_set(DeviceAction, 5), ", "),
    Protocols = strcat_array(make_set(Protocol, 5), ", "),
    DeviceProducts = strcat_array(make_set(DeviceProduct, 5), ", "),
    DeviceVendors = strcat_array(make_set(DeviceVendor, 5), ", "),
    Devices = strcat_array(make_set(DeviceName, 5), ", "),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated)
by SourceIP
| where EventCount > 5
| where UniqueDestinations > 200
| project
    StartTime,
    EndTime,
    SourceIP,
    EventCount,
    UniqueDestinations,
    DestinationPorts,
    Protocols,
    DeviceActions,
    DeviceVendors,
    DeviceProducts,
    Devices,
    DestinationIPs

19.	MULTIPLE LOGIN FAILURES FROM THE SAME SOURCE  CONTAINING LOGONERROR-USERSTRONGAUTHCLIENTAUTHNREQUIREDINTERRUPT

Description
This analytic rule detects potential password spraying or brute-force attacks originating from a single internal source IP address targeting multiple different user accounts within a short time window. The query analyses authentication logs ingested into the CommonSecurityLog table and identifies cases where a source IP generates 10 or more authentication failure events against 3 or more distinct usernames within a 5-minute window. Events sourced from Azure Active Directory are excluded to avoid duplication with Azure AD Identity Protection. Known organizational public IPs, Amazon Technologies IPs, and Domain Controller IPs are excluded via watchlists to suppress known false positives. This behaviour is characteristic of automated credential spraying tools attempting to gain unauthorized access while evading account lockout thresholds.
MITRE ATT&CK Mapping
Tactic:
•	Initial Access
•	Credential Access
Techniques:
•	T1110.003 – Password Spraying 
•	T1110.001 – Password Guessing 
•	T1078 – Valid Accounts 
•	T1133 – External Remote Services 

Query:
// Step 1: Load exclusion watchlists
let AMK_PublicIPs =
    _GetWatchlist('AMK_PublicIP')
    | project ExcludedIP = tostring(SearchKey);
let AmazonIPs =
    _GetWatchlist('External_AmazonIP')
    | project ExcludedIP = tostring(SearchKey);
let DomainControllerIPs =
    _GetWatchlist('DomainControllers_IP')
    | project ExcludedIP = tostring(SearchKey);
let ExcludedIPs =
    union AMK_PublicIPs, AmazonIPs, DomainControllerIPs
    | distinct ExcludedIP;
// Step 2: Filter authentication failure events from CommonSecurityLog
CommonSecurityLog
| where TimeGenerated >= ago(5m)
// Auth failures only — maps to BB:CategoryDefinition: Authentication Failures
| where Activity has_any (
    "Authentication Failure",
    "Failed Authentication",
    "Login Failed",
    "Logon Failure",
    "Failed password",
    "Invalid credentials",
    "Authentication failed"
  )
  or DeviceEventClassID has_any ("authentication-failed", "login-failed", "auth-failure")
// Exclude Azure Active Directory sourced events
| where DeviceVendor !in ("Microsoft", "Azure")
  or DeviceProduct !has "Azure Active Directory"
// Only internal source IPs (not public network sources)
| where ipv4_is_private(SourceIP)
// Exclude empty or loopback source IPs
| where isnotempty(SourceIP)
| where SourceIP !in ("127.0.0.1", "0.0.0.0", "::")
// Step 3: Apply watchlist-based IP exclusions
| where SourceIP !in (ExcludedIPs)
// Step 4: Normalize username — exclude machine/service accounts
| extend NormalizedUser = tolower(trim(@"\s+", DestinationUserName))
| where isnotempty(NormalizedUser)
| where not(NormalizedUser endswith "$")       // Exclude machine accounts
| where NormalizedUser !in ("anonymous", "n/a", "-", "unknown")
// Step 5: Aggregate within 5-minute bins per source IP
| summarize
    TotalFailures       = count(),
    DistinctUsernames   = dcount(NormalizedUser),
    UsernameList        = make_set(NormalizedUser, 50),
    DistinctDestIPs     = dcount(DestinationIP),
    DestinationIPs      = make_set(DestinationIP, 20),
    DestinationPorts    = make_set(DestinationPort, 20),
    Actions             = make_set(DeviceAction, 10),
    Vendors             = make_set(DeviceVendor, 5),
    Products            = make_set(DeviceProduct, 5),
    Activities          = make_set(Activity, 10),
    FirstSeen           = min(TimeGenerated),
    LastSeen            = max(TimeGenerated)
    by SourceIP, bin(TimeGenerated, 5m)
// Step 6: Apply thresholds — 10+ failures across 3+ distinct usernames
| where TotalFailures >= 10
| where DistinctUsernames >= 3
// Step 7: Project final alert fields
| project
    TimeGenerated,
    SourceIP                = tostring(SourceIP),
    TotalFailures           = tostring(TotalFailures),
    DistinctUsernames       = tostring(DistinctUsernames),
    DistinctDestIPs         = tostring(DistinctDestIPs),
    UsernameList            = strcat_array(UsernameList, ", "),
    DestinationIPs          = strcat_array(DestinationIPs, ", "),
    DestinationPorts        = strcat_array(DestinationPorts, ", "),
    Actions                 = strcat_array(Actions, ", "),
    Vendors                 = strcat_array(Vendors, ", "),
    Products                = strcat_array(Products, ", "),
    Activities              = strcat_array(Activities, ", "),
    FirstSeen,
    LastSeen
| sort by TotalFailures desc
  

Watchlists Required
Before deploying this rule, ensure these three watchlists exist in Sentinel → Watchlists:


Watchlist Name	Contents	Purpose
AMK_PublicIP	Your org's known public IP ranges	Prevent alerts on own infrastructure
External_AmazonIP	Amazon Technologies IP ranges	Suppress AWS retry auth noise
DomainControllers_IP	All DC IP addresses in your environment	Prevent DC Kerberos recursive auth from triggering

20.	InfoSec: Vera Restricted Location 

Description
This analytic rule detects file activity originating from geographically restricted or high-risk countries as defined by your organization's security policy. The query analyzes logs from Vera (now Broadcom Information-Centric Security — a data-centric security and DRM platform that tracks file access and sharing activity) and cross-references the source country of each event against a watchlist of restricted locations maintained by the security team. Any file access, sharing, download, or modification event where the originating country matches the restricted list generates an alert. This rule is critical for detecting data exfiltration attempts, policy violations, or unauthorized access to sensitive documents from sanctioned or high-risk geographies.
MITRE ATT&CK Mapping
Tactics:
•	Collection
•	Exfiltration
•	Initial Access
•	Defense Evasion
Techniques:
•	T1530 – Data from Cloud Storage Object
•	T1567 – Exfiltration Over Web Service
•	T1048 – Exfiltration Over Alternative Protocol
•	T1078 – Valid Accounts
•	T1566 – Phishing
•	T1550 – Use Alternate Authentication Material

Query:
// Step 1: Load restricted country watchlist
// Equivalent of "UBA: Restricted Location List" from QRadar
let RestrictedCountries =
    _GetWatchlist('Vera_RestrictedLocations')
    | project Country = tolower(trim(@"\s+", tostring(SearchKey)));
// Step 2: Pull Vera events from CommonSecurityLog
CommonSecurityLog
| where TimeGenerated >= ago(5m)
// Scope to Vera logs only
// Equivalent of "detected by Vera" in QRadar rule
| where DeviceVendor has_any ("Vera", "Broadcom")
  or DeviceProduct has_any ("Vera", "Information-Centric Security", "ICS")
// Exclude events with no location data
| where isnotempty(SourceIP)
// Step 3: Extract and normalize country field
// Vera logs country in DeviceCustomString fields or AdditionalExtensions
| extend Country_Raw = coalesce(
    tostring(DeviceCustomString1),       // Check custom string fields
    tostring(DeviceCustomString2),
    tostring(DeviceCustomString3),
    extract(@"country=([^;|,]+)", 1, AdditionalExtensions),
    extract(@"src_country=([^;|,]+)", 1, AdditionalExtensions),
    extract(@"c=([^;|,]+)", 1, AdditionalExtensions)
  )
| extend Country_Normalized = tolower(trim(@"\s+", Country_Raw))
// Exclude events with no country data
| where isnotempty(Country_Normalized)
| where Country_Normalized !in ("unknown", "-", "n/a", "")
// Step 4: Match against restricted country watchlist
| join kind=inner (
    RestrictedCountries
) on $left.Country_Normalized == $right.Country
// Step 5: Enrich with available Vera-specific fields
| extend
    FileName        = coalesce(
                        tostring(DeviceCustomString4),
                        extract(@"fname=([^;|,]+)", 1, AdditionalExtensions),
                        RequestURL
                      ),
    FileOwner       = coalesce(
                        tostring(DestinationUserName),
                        tostring(SourceUserName),
                        extract(@"suser=([^;|,]+)", 1, AdditionalExtensions)
                      ),
    ActionTaken     = coalesce(
                        tostring(DeviceAction),
                        tostring(Activity)
                      ),
    PolicyViolated  = extract(@"policy=([^;|,]+)", 1, AdditionalExtensions)
// Step 6: Aggregate per user and country within 5-min window
| summarize
    TotalEvents         = count(),
    DistinctFiles       = dcount(FileName),
    FileList            = make_set(FileName, 30),
    DistinctSourceIPs   = dcount(SourceIP),
    SourceIPs           = make_set(SourceIP, 10),
    Actions             = make_set(ActionTaken, 10),
    Policies            = make_set(PolicyViolated, 10),
    Products            = make_set(DeviceProduct, 5),
    Activities          = make_set(Activity, 10),
    FirstSeen           = min(TimeGenerated),
    LastSeen            = max(TimeGenerated)
    by
    SourceUser      = FileOwner,
    RestrictedCountry   = Country_Normalized,
    bin(TimeGenerated, 5m)
// Step 7: Project final alert output
| project
    TimeGenerated,
    SourceUser,
    RestrictedCountry,
    TotalEvents             = tostring(TotalEvents),
    DistinctFiles           = tostring(DistinctFiles),
    DistinctSourceIPs       = tostring(DistinctSourceIPs),
    FileList                = strcat_array(FileList, ", "),
    SourceIPs               = strcat_array(SourceIPs, ", "),
    Actions                 = strcat_array(Actions, ", "),
    Policies                = strcat_array(Policies, ", "),
    Activities              = strcat_array(Activities, ", "),
    FirstSeen,
    LastSeen
| sort by TotalEvents desc
 

WatchList Required:
Watchlist Name	Contents	Example Entries
Vera_RestrictedLocations	Country names or codes your org restricts	Russia, China, Iran, North Korea

21.	InfoSec: Local L2L DNS Scanner

Description
This analytic rule detects potential internal DNS-based reconnaissance activity where a single internal source host sends an abnormally high volume of DNS traffic to more than 59 unique internal destination IP addresses within a 10-minute window. The query analyzes firewall and network logs ingested into the CommonSecurityLog table and identifies events matching DNS port definitions (UDP/TCP port 53) where the activity is classified under reconnaissance or suspicious event categories. The rule focuses exclusively on LAN to LAN (L2L) traffic — both source and destination must be private/internal IPs. Known DNS servers, vulnerability assessment scanners, authorized scanning IPs, and Domain Controllers are excluded via watchlists to suppress legitimate high-volume DNS activity. This behavior is indicative of automated internal network discovery tools, malware performing lateral movement preparation, or a compromised host mapping the internal network topology.
MITRE ATT&CK Mapping 
Tactics:
•	Discovery
•	Reconnaissance
Techniques:
•	T1046 – Network Service Discovery
•	T1018 – Remote System Discovery 
•	T1590.002 – Gather Victim Network Information: DNS 
•	T1071.004 – Application Layer Protocol: DNS
Query:
// Step 1: Load exclusion watchlists
let DNS_Servers =
    _GetWatchlist('DNS_Servers')
    | project ExcludedIP = tostring(SearchKey);
let VA_Scanners =
    _GetWatchlist('VA_Scanners')
    | project ExcludedIP = tostring(SearchKey);
let ValidScanners =
    _GetWatchlist('Valid_Scanning_IP')
    | project ExcludedIP = tostring(SearchKey);
let DomainControllers =
    _GetWatchlist('DomainControllers_IP')
    | project ExcludedIP = tostring(SearchKey);
let ExcludedIPs =
    union DNS_Servers, VA_Scanners, ValidScanners, DomainControllers
    | distinct ExcludedIP;
// Step 2: Define DNS ports
// BB:PortDefinition: DNS Ports = 53 (UDP/TCP)
let DNS_Ports = dynamic([53, 5353, 5355]);  
// 5353 = mDNS, 5355 = LLMNR — include if relevant in your environment
// Step 3: Pull DNS-related events from CommonSecurityLog
CommonSecurityLog
| where TimeGenerated >= ago(10m)
// L2L context — both source and destination must be internal
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
// Exclude empty IPs
| where isnotempty(SourceIP)
| where isnotempty(DestinationIP)
// BB:PortDefinition: DNS Ports
| where DestinationPort in (DNS_Ports)
  or ApplicationProtocol has_any ("dns", "mdns", "llmnr", "domain")
// Exclude known DNS servers as SOURCE
// (DNS servers legitimately query many destinations)
| where SourceIP !in (ExcludedIPs)
// Exclude known DNS servers as DESTINATION
// (Normal client-to-DNS-server traffic is expected)
| where DestinationIP !in (ExcludedIPs)
// Step 4: Focus on Recon and Suspicious event categories
// BB:CategoryDefinition: Recon Events + Suspicious Events
| where DeviceEventClassID has_any (
    "recon", "scan", "probe", "sweep",
    "suspicious", "anomaly", "policy-violation"
  )
  or Activity has_any (
    "recon", "scan", "probe", "dns scan",
    "suspicious", "zone transfer", "dns sweep",
    "network scan", "host discovery"
  )
  or DeviceAction has_any (
    "drop", "deny", "block", "reject"     
    // Blocked DNS to many hosts = scan behavior
  )
// Step 5: Aggregate per source IP in 10-minute bins
| summarize
    hint.shufflekey = SourceIP
    TotalEvents         = count(),
    UniqueDestIPs       = dcount(DestinationIP),
    DestIPs             = make_set(DestinationIP, 100),
    DestPorts           = make_set(DestinationPort, 10),
    Protocols           = make_set(Protocol, 5),
    Applications        = make_set(ApplicationProtocol, 10),
    Actions             = make_set(DeviceAction, 10),
    Categories          = make_set(DeviceEventClassID, 10),
    Activities          = make_set(Activity, 10),
    Vendors             = make_set(DeviceVendor, 5),
    Products            = make_set(DeviceProduct, 5),
    FirstSeen           = min(TimeGenerated),
    LastSeen            = max(TimeGenerated)
    by SourceIP, bin(TimeGenerated, 10m)
// Step 6: Apply thresholds
// More than 5 events across more than 59 destination IPs
| where TotalEvents > 5
| where UniqueDestIPs > 59
// Step 7: Project final alert output
| project
    TimeGenerated,
    SourceIP                = tostring(SourceIP),
    TotalEvents             = tostring(TotalEvents),
    UniqueDestIPs           = tostring(UniqueDestIPs),
    DestIPs                 = strcat_array(DestIPs, ", "),
    DestPorts               = strcat_array(DestPorts, ", "),
    Protocols               = strcat_array(Protocols, ", "),
    Applications            = strcat_array(Applications, ", "),
    Actions                 = strcat_array(Actions, ", "),
    Categories              = strcat_array(Categories, ", "),
    Activities              = strcat_array(Activities, ", "),
    Vendors                 = strcat_array(Vendors, ", "),
    Products                = strcat_array(Products, ", "),
    FirstSeen,
    LastSeen
| sort by UniqueDestIPs desc
 

Watchlist Name	Contents	Example Entries
DNS_Servers	All internal DNS server IPs	10.0.0.53, 10.0.1.53
VA_Scanners	Vulnerability assessment scanner IPs	Nessus, Qualys scanner IPs
Valid_Scanning_IP	Authorized internal scanning tool IPs	Approved pen test IPs
DomainControllers_IP	All internal Domain Controller IPs	10.0.0.1, 10.0.0.2

22.	Access Token Abuse

Description
This analytic rule detects token impersonation and theft activity on Windows systems by monitoring Security Event Log logon events. Specifically it targets Logon Type 5 (Service logons) combined with Advapi logon process, Negotiate authentication package, and Impersonation-level impersonation — a combination that indicates abuse of Windows token APIs such as DuplicateToken(Ex) and ImpersonateLoggedOnUser with the LOGON32_LOGON_NEW_CREDENTIALS flag. This technique allows attackers to impersonate other users or escalate privileges without knowing their credentials by duplicating or stealing access tokens from running processes.

KQL Query
SecurityEvent
| where TimeGenerated >= ago(7d)
| where EventID == 4624
| where LogonType == 5
| where LogonProcessName == "Advapi"
| where AuthenticationPackageName == "Negotiate"
| where ImpersonationLevel == "%%1833"
| project
    TimeGenerated,
    Computer,
    TargetUserName,
    TargetDomainName,
    LogonType,
    LogonTypeName,
    LogonProcessName,
    AuthenticationPackageName,
    ImpersonationLevel,
    IpAddress,
    ProcessName,
    SubjectUserName,
    SubjectDomainName,
    WorkstationName,
    SubjectLogonId,
    TargetLogonId,
    LogonGuid,
    TokenElevationType
| sort by TimeGenerated desc 
 

MITRE ATT&CK Mapping
•	Tactics:
•	Privilege Escalation
•	Defense Evasion
•	Lateral Movement
•	Techniques:
•	T1134.001 – Access Token Manipulation: Token Impersonation/Theft
•	T1134.002 – Access Token Manipulation: Create Process with Token
•	T1078 – Valid Accounts
•	T1550.003 – Use Alternate Authentication Material: Pass the Token


23.	Cryptocurrency Mining Command Execution 

Description
This analytic rule detects potential cryptocurrency mining activity on endpoints by monitoring process creation and command execution events on Windows systems. The rule identifies processes or command line arguments containing keywords associated with known cryptocurrency miners and coins such as miner, coinhive, cryptonight, monero, bitcoin, ethereum, and others. This behavior may indicate a host infected with cryptomining malware or an employee misusing corporate assets for personal cryptocurrency mining. A process exception watchlist is included to suppress known legitimate processes that may contain matching keywords.

Query
// Crypto mining keyword list
let CryptoKeywords = dynamic([
    "miner", "coin", "coinhive", "cryptonight",
    "ethereum", "xrp", "stellar", "monero", "bitcoin"
]);
// Process exception watchlist
let ExceptionList =
    _GetWatchlist('Process_Exception_Cryptomining')
    | project ProcessPath = tostring(SearchKey);
SecurityEvent
| where TimeGenerated >= ago(5m)
// Process creation events only
| where EventID in (4688, 1)
// Must have command line data
| where isnotempty(CommandLine) or isnotempty(NewProcessName)
| where TimeGenerated >= ago(5m)
// Process creation events only
| where EventID in (4688, 1)
// Must have command line data
| where isnotempty(CommandLine) or isnotempty(NewProcessName)
// Exclude whitelisted process paths
| where NewProcessName !in (ExceptionList)
// Match crypto keywords in command line or process name
| where CommandLine matches regex @"(?i).*(miner|coin|coinhive|cryptonight|ethereum|xrp|stellar|monero|bitcoin).*"
       or NewProcessName matches regex @"(?i).*(miner|coin|coinhive|cryptonight
 
// Exclude whitelisted process paths
| where NewProcessName !in (ExceptionList)
// Match crypto keywords in command line or process name
| where CommandLine matches regex @"(?i).*(miner|coin|coinhive|cryptonight|ethereum|xrp|stellar|monero|bitcoin).*"
       or NewProcessName matches regex @"(?i).*(miner|coin|coinhive|cryptonight|ethereum|xrp|stellar|monero|bitcoin).*"
| project
    TimeGenerated,
    Computer,
    SubjectUserName,
    SubjectDomainName,
    NewProcessName,
    NewProcessId,
    ParentProcessName,
    CommandLine,
    TokenElevationType,
    LogonGuid,
    SubjectLogonId,
    WorkstationName
| sort by TimeGenerated desc
 

Mitre Attack:
Tactics:
•	Execution
•	Impact
•	Persistence
•	Defense Evasion
Techniques:
•	T1059 – Command and Scripting Interpreter
•	T1496 – Resource Hijacking
•	T1204.002 – User Execution: Malicious File
•	T1053 – Scheduled Task/Job
•	T1036 – Masquerading

 24.	Remote: Possible Tunneling

Description
This analytic rule detects potential network tunneling activity by identifying abnormally large ICMP or DNS packets being transmitted to external destinations. Tunneling techniques abuse ICMP or DNS protocols to encapsulate and exfiltrate data or establish covert command and control channels that bypass traditional security controls. The rule monitors for at least 10 occurrences of large ICMP or large DNS packets from the same source and destination IP pair within a 10-minute window. Google DNS servers and Domain Controller DNS traffic are explicitly excluded to reduce false positives. Any traffic that is not LAN to LAN is in scope ensuring only outbound or external communication is analysed.

Query

let ExcludedDNS = dynamic(["8.8.8.8", "8.8.4.4"]);
let DomainControllers =
    _GetWatchlist('DomainControllers_IP')
    | project ControllerIP = tostring(SearchKey);
// Thresholds for large packets — tunable
let LargeICMP = 1000;  // bytes
let LargeDNS  = 512;   // bytes — standard DNS max is 512 bytes
CommonSecurityLog
| where TimeGenerated >= ago(10m)
// Exclude L2L — at least one side must be external
| where not(ipv4_is_private(SourceIP) and ipv4_is_private(DestinationIP))
// Exclude Google DNS
| where DestinationIP !in (ExcludedDNS)
// Exclude DC as source on port 53
| where not(SourceIP in (DomainControllers) and DestinationPort == 53)
// Focus on ICMP and DNS protocols only
| where Protocol in ("ICMP", "UDP", "TCP")
| where ApplicationProtocol has_any ("icmp", "dns") 
      or DestinationPort == 53
      or Protocol == "ICMP"
// Large packet detection
| where (Protocol == "ICMP" and (ReceivedBytes > LargeICMP or SentBytes > LargeICMP))
      or (DestinationPort == 53 and (ReceivedBytes > LargeDNS or SentBytes > LargeDNS))
// Successful communication only
| where DeviceAction !in ("deny", "drop", "block", "reject")
| summarize
    hint.shufflekey = SourceIP
    EventCount          = count(),
    TotalBytesSent      = sum(SentBytes),
    TotalBytesReceived  = sum(ReceivedBytes),
    AvgPacketSize       = avg(ReceivedBytes + SentBytes),
    Protocols           = make_set(Protocol, 5),
    Applications        = make_set(ApplicationProtocol, 5),
    DestPorts           = make_set(DestinationPort, 10),
    Actions             = make_set(DeviceAction, 5),
    Vendors             = make_set(DeviceVendor, 5),
    Products            = make_set(DeviceProduct, 5),
    FirstSeen           = min(TimeGenerated),
    LastSeen            = max(TimeGenerated)
    by SourceIP, DestinationIP, bin(TimeGenerated, 10m)
// At least 10 large packet events same src/dst pair
| where EventCount >= 10
| project
    TimeGenerated,
    SourceIP,
    DestinationIP,
    EventCount          = tostring(EventCount),
    TotalBytesSent      = tostring(TotalBytesSent),
    TotalBytesReceived  = tostring(TotalBytesReceived),
    AvgPacketSize       = tostring(AvgPacketSize),
    Protocols           = strcat_array(Protocols, ", "),
    Applications        = strcat_array(Applications, ", "),
    DestPorts           = strcat_array(DestPorts, ", "),
    Actions             = strcat_array(Actions, ", "),
    Vendors             = strcat_array(Vendors, ", "),
    Products            = strcat_array(Products, ", "),
    FirstSeen,
    LastSeen
| sort by EventCount desc
 

Watchlists Required
Watchlist Name	Contents	Example Entries
DomainControllers_IP	All internal Domain Controller IPs	10.0.0.1, 10.0.0.2

MITRE ATT&CK Mapping
Tactics:
•	Command and Control
•	Exfiltration
•	Defense Evasion
Techniques:
•	T1095 – Non-Application Layer Protocol
•	T1071.004 – Application Layer Protocol: DNS
•	T1048.003 – Exfiltration Over Alternative Protocol
•	T1572 – Protocol Tunneling
•	T1041 – Exfiltration Over C2 Channel


25.	Database Remote Login Success

Description
This analytic rule detects successful remote authentication events to internal database servers. The rule monitors Windows Security Event logs for successful logon events where the source is a remote network address and the destination host is a known database server as defined in the database servers watchlist. Unauthorized or unexpected remote logins to database servers may indicate credential compromise, lateral movement, or an attacker attempting to access sensitive data stores directly. This rule helps ensure that only approved users and systems are authenticating to critical database infrastructure from remote locations.

Query

let DatabaseServers =
    _GetWatchlist('Database_Servers')
    | project ServerName = tolower(tostring(SearchKey));
SecurityEvent
| where TimeGenerated >= ago(5m)
| where EventID == 4624
// Successful remote logon types only
// Type 3 = Network, Type 10 = RemoteInteractive
| where LogonType in (3, 10)
// Must come from a remote IP — exclude local/loopback
| where isnotempty(IpAddress)
| where IpAddress !in ("127.0.0.1", "::1", "-", "0.0.0.0")
// Scope to database servers only
| where tolower(Computer) in (DatabaseServers)
// Exclude machine accounts and system accounts
| where SubjectUserName !endswith "$"
| where TargetUserName !endswith "$"
| where TargetUserName !in ("SYSTEM", "LOCAL SERVICE", "NETWORK SERVICE", "ANONYMOUS LOGON")
| project
    TimeGenerated,
    Computer,
    TargetUserName,
    TargetDomainName,
    SubjectUserName,
    SubjectDomainName,
    LogonType,
    LogonTypeName,
    LogonProcessName,
    AuthenticationPackageName,
    IpAddress,
    IpPort,
    WorkstationName,
    LogonGuid,
    TargetLogonId,
    SubjectLogonId
| sort by TimeGenerated desc
 

Watchlists Required

Watchlist Name	Contents	Example Entries
Database_Servers	Hostnames or IPs of all database servers	SQLPROD01, ORADB02, 10.0.5.20
________________________________________

MITRE ATT&CK Mapping
Tactics:
•	Initial Access
•	Lateral Movement
•	Collection
Techniques:
•	T1078 – Valid Accounts
•	T1021.001 – Remote Services: Remote Desktop Protocol
•	T1021.002 – Remote Services: SMB/Windows Admin Shares
•	T1213 – Data from Information Repositories
•	T1190 – Exploit Public-Facing Application
 26.	Palo Alto Firewall Rule Modified to Allow Any Destination

Description – 
This analytic rule detects when a Palo Alto firewall security rule is modified such that its destination is changed from a restricted set of objects to "any".
Such changes significantly broaden network access and may indicate misconfiguration, policy weakening, or malicious activity. This behavior can lead to unintended exposure of internal resources, increased attack surface, and potential lateral movement.
The rule analyzes configuration change logs and compares the previous and updated rule definitions to identify when destination constraints are removed and replaced with "any".
Alert context includes the user who performed the change, originating host, firewall device, device group, rule name, and before/after destination values to support rapid investigation.

Query:
CommonSecurityLog
| where Activity contains "config"
| where DeviceAction == "edit"
| where isnotempty(DeviceCustomString1) and isnotempty(DeviceCustomString2)
| extend 
    AfterChange = DeviceCustomString1,
    BeforeChange = DeviceCustomString2
// BEFORE parsing
| extend 
    RuleName_Before = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, BeforeChange),
    RuleUUID_Before = extract(@"([a-f0-9\-]{36})", 1, BeforeChange),
    Source_Before = extract(@"source\s+\[\s*([^\]]+)\]", 1, BeforeChange),
    Destination_Before = extract(@"destination\s+\[\s*([^\]]+)\]", 1, BeforeChange),
    Description_Before = extract(@"description\s+([^}]+)", 1, BeforeChange)
// AFTER parsing
| extend 
    RuleName_After = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, AfterChange),
    RuleUUID_After = extract(@"([a-f0-9\-]{36})", 1, AfterChange),
    Source_After = extract(@"source\s+\[\s*([^\]]+)\]", 1, AfterChange),
    Destination_After = extract(@"destination\s+\[\s*([^\]]+)\]", 1, AfterChange),
    Description_After = extract(@"description\s+([^}]+)", 1, AfterChange)
// 🔥 Detection Logic
| where isnotempty(Destination_Before) and isnotempty(Destination_After)
| where tolower(trim(" ", Destination_Before)) != tolower(trim(" ", Destination_After))
| where tolower(Destination_Before) !contains "any"
| where tolower(Destination_After) contains "any"
// 🔍 Extract Device Group
| extend DeviceGroup = extract(@"PanOSFWDeviceGroup=([^;]+)", 1, AdditionalExtensions)
// ✅ Correct Identity Mapping
| extend 
    Username = DestinationUserName,
    OriginHost = SourceHostName,
    Firewall = DeviceName
// Normalize rule info
| extend 
    RuleName = coalesce(RuleName_After, RuleName_Before),
    RuleID = coalesce(RuleUUID_After, RuleUUID_Before)
// Final output
| project 
    TimeGenerated,
    Username,
    OriginHost,
    Firewall,
    DeviceGroup,
    RuleName,
    RuleID,
    Destination_Before,
    Destination_After,
    Description_Before,
    Description_After,
    Message

MITRE ATT&CK Mapping
🎯 Primary Technique
•	T1562.004 Impair Defenses: Disable or Modify System Firewall 
👉 Reason:
•	Firewall rule weakened → security control bypass 
🎯 Secondary Techniques (Contextual Impact)
•	T1021 Remote Services
→ Broader access may enable lateral movement 
•	T1046 Network Service Discovery
→ Expanded access enables reconnaissance 
🎯 MITRE Tactics
Map these in Sentinel:
•	Defense Evasion 
•	Lateral Movement (secondary) 
•	Discovery (secondary)

 27.	Palo Alto Firewall Rule Modified to Allow Any Source

Description :
This analytic rule detects when a Palo Alto firewall security rule is modified such that its source is changed from a restricted set of objects to "any".
This type of change significantly broadens access by allowing traffic from any source, which can increase the attack surface and introduce risk of unauthorized access, lateral movement, or external exposure.
The rule analyzes configuration change logs and compares the previous and updated rule definitions to identify when source restrictions are removed and replaced with "any".
Alert context includes the user who performed the change, originating host, firewall device, device group, rule name, and before/after source values to support investigation and auditing.

Query:
CommonSecurityLog
| where Activity contains "config"
| where DeviceAction == "edit"
| where isnotempty(DeviceCustomString1) and isnotempty(DeviceCustomString2)
| extend AfterChange = DeviceCustomString1,
BeforeChange = DeviceCustomString2 // BEFORE parsing
| extend
RuleName_Before = extract(@"^([^{]+?)\s+[a-f0-9-]{36}", 1, BeforeChange),
RuleUUID_Before = extract(@"([a-f0-9-]{36})", 1, BeforeChange),
Source_Before = extract(@"source\s+[\s*([^]]+)]", 1, BeforeChange),
Destination_Before = extract(@"destination\s+[\s*([^]]+)]", 1, BeforeChange)
// AFTER parsing
| extend
RuleName_After = extract(@"^([^{]+?)\s+[a-f0-9-]{36}", 1, AfterChange),
RuleUUID_After = extract(@"([a-f0-9-]{36})", 1, AfterChange),
Source_After = extract(@"source\s+[\s*([^]]+)]", 1, AfterChange),
Destination_After = extract(@"destination\s+[\s*([^]]+)]", 1, AfterChange)
// 🔥 Detection Logic (SOURCE → ANY)
| where isnotempty(Source_Before) and isnotempty(Source_After)
| where tolower(trim(" ", Source_Before)) != tolower(trim(" ", Source_After))
| where tolower(Source_Before) !contains "any"
| where tolower(Source_After) contains "any"
// Extract Device Group
| extend DeviceGroup = extract(@"PanOSFWDeviceGroup=([^;]+)", 1, AdditionalExtensions)
// Identity Mapping
| extend
Username = DestinationUserName,
OriginHost = SourceHostName,
Firewall = DeviceName
// Normalize rule info
| extend
RuleName = coalesce(RuleName_After, RuleName_Before),
RuleID = coalesce(RuleUUID_After, RuleUUID_Before)
// Final output
| project
TimeGenerated,
Username,
OriginHost,
Firewall,
DeviceGroup,
RuleName,
RuleID,
Source_Before,
Source_After,
Message

MITRE ATT&CK Mapping
🎯 Primary Technique
•	T1562.004 Impair Defenses: Disable or Modify System Firewall 
🎯 Secondary Techniques
•	T1021 Remote Services 
•	T1046 Network Service Discovery 
🎯 Tactics
•	Defense Evasion 
•	Lateral Movement 
•	Discovery

28.	Palo Alto Firewall Rule Modified to Add New Source IP or Object

Description:
This analytic rule detects when a Palo Alto firewall security rule is modified to include new source IP addresses or objects.
Such changes may expand access to the rule by allowing additional systems, networks, or groups to initiate traffic. Depending on the context, this could indicate unauthorized access expansion, misconfiguration, or policy drift.
The rule compares the previous and updated rule definitions to identify newly introduced source elements. It provides visibility into what was added, along with the associated rule, user, and device context to support investigation.
Not all detections indicate malicious behavior; some may represent legitimate configuration updates. However, unexpected or unapproved additions should be reviewed to ensure compliance with security policies.
 Query:
CommonSecurityLog
| where Activity contains "config"
| where DeviceAction == "edit"
| where isnotempty(DeviceCustomString1) and isnotempty(DeviceCustomString2)
| extend 
    AfterChange = DeviceCustomString1,
    BeforeChange = DeviceCustomString2
// BEFORE parsing
| extend 
    RuleName_Before = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, BeforeChange),
    RuleUUID_Before = extract(@"([a-f0-9\-]{36})", 1, BeforeChange),
    Source_Before = extract(@"source\s+\[\s*([^\]]+)\]", 1, BeforeChange)
// AFTER parsing
| extend 
    RuleName_After = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, AfterChange),
    RuleUUID_After = extract(@"([a-f0-9\-]{36})", 1, AfterChange),
    Source_After = extract(@"source\s+\[\s*([^\]]+)\]", 1, AfterChange)
// Ensure valid data
| where isnotempty(Source_Before) and isnotempty(Source_After)
| where trim(" ", Source_Before) != trim(" ", Source_After)
// 🔥 Convert to arrays
| extend 
    SrcBeforeList = split(Source_Before, " "),
    SrcAfterList = split(Source_After, " ")
// 🔥 Find NEW elements using set_difference
| extend AddedSources = set_difference(SrcAfterList, SrcBeforeList)
// Keep only real additions
| where array_length(AddedSources) > 0
// Expand results
| mv-expand AddedSources
| extend AddedSource = trim(" ", tostring(AddedSources))
// Identity + context
| extend 
    Username = DestinationUserName,
    OriginHost = SourceHostName,
    Firewall = DeviceName,
    DeviceGroup = extract(@"PanOSFWDeviceGroup=([^;]+)", 1, AdditionalExtensions)
// Normalize rule info
| extend 
    RuleName = coalesce(RuleName_After, RuleName_Before),
    RuleID = coalesce(RuleUUID_After, RuleUUID_Before)
// Final output
| project 
    TimeGenerated,
    Username,
    OriginHost,
    Firewall,
    DeviceGroup,
    RuleName,
    RuleID,
    AddedSource,
    Source_Before,
    Source_After,
    Message
 MITRE ATT&CK Mapping
🎯 Primary Technique
T1562.004 Impair Defenses: Disable or Modify System Firewall 
👉 Reason:
Firewall rule modified → access control altered 
🎯 Secondary Techniques
•	T1021 Remote Services
👉 New sources may enable remote access paths 
•	T1133 External Remote Services
👉 If external IPs are added 
🎯 MITRE Tactics
•	Defense Evasion 
•	Lateral Movement 
•	Initial Access (conditional)

29.	Palo Alto Firewall Rule Modified to Add New Destination IP or Object

Description:
This analytic rule detects when a Palo Alto firewall security rule is modified to include new destination IP addresses or objects.
Such changes expand the scope of accessible systems by allowing traffic to additional destinations, which may introduce risk of unauthorized access, lateral movement, or exposure of sensitive assets such as servers or databases.
The rule compares previous and updated rule configurations to identify newly introduced destination elements. It provides visibility into what was added, along with user, device, and rule context to support investigation and auditing.
While some changes may be legitimate, unexpected or unapproved destination additions should be reviewed to ensure compliance with security policies and to prevent unintended exposure.
Query:
CommonSecurityLog
| where Activity contains "config"
| where DeviceAction == "edit"
| where isnotempty(DeviceCustomString1) and isnotempty(DeviceCustomString2)
| extend 
    AfterChange = DeviceCustomString1,
    BeforeChange = DeviceCustomString2
// BEFORE parsing
| extend 
    RuleName_Before = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, BeforeChange),
    RuleUUID_Before = extract(@"([a-f0-9\-]{36})", 1, BeforeChange),
    Source_Before = extract(@"source\s+\[\s*([^\]]+)\]", 1, BeforeChange)
// AFTER parsing
| extend 
    RuleName_After = extract(@"^([^\{]+?)\s+[a-f0-9\-]{36}", 1, AfterChange),
    RuleUUID_After = extract(@"([a-f0-9\-]{36})", 1, AfterChange),
    Source_After = extract(@"source\s+\[\s*([^\]]+)\]", 1, AfterChange)
// Ensure valid data
| where isnotempty(Source_Before) and isnotempty(Source_After)
| where trim(" ", Source_Before) != trim(" ", Source_After)
// 🔥 Convert to arrays
| extend 
    SrcBeforeList = split(Source_Before, " "),
    SrcAfterList = split(Source_After, " ")
// 🔥 Find NEW elements using set_difference
| extend AddedSources = set_difference(SrcAfterList, SrcBeforeList)
// Keep only real additions
| where array_length(AddedSources) > 0
// Expand results
| mv-expand AddedSources
| extend AddedSource = trim(" ", tostring(AddedSources))
// Identity + context
| extend 
    Username = DestinationUserName,
    OriginHost = SourceHostName,
    Firewall = DeviceName,
    DeviceGroup = extract(@"PanOSFWDeviceGroup=([^;]+)", 1, AdditionalExtensions)
// Normalize rule info
| extend 
    RuleName = coalesce(RuleName_After, RuleName_Before),
    RuleID = coalesce(RuleUUID_After, RuleUUID_Before)
// Final output
| project 
    TimeGenerated,
    Username,
    OriginHost,
    Firewall,
    DeviceGroup,
    RuleName,
    RuleID,
    AddedSource,
    Source_Before,
    Source_After,
    Message

MITRE ATT&CK Mapping
🎯 Primary Technique
•	T1562.004 Impair Defenses: Disable or Modify System Firewall 
🎯 Secondary Techniques
•	T1021 Remote Services
👉 Expanded destination access enables lateral movement paths 
•	T1046 Network Service Discovery
👉 New destinations may be leveraged for discovery 
🎯 MITRE Tactics
•	Defense Evasion 
•	Lateral Movement 
•	Discovery 

30.	Palo Alto Firewall – Multiple Rule Changes by Same User (20 mins window)

Description:
This analytic rule detects when a user performs multiple Palo Alto firewall configuration changes within a short time window.
A high frequency of rule modifications in a limited period may indicate bulk configuration changes, automation activity, misconfiguration, or potentially unauthorized or malicious behavior. Rapid changes can increase the risk of security gaps, policy misalignment, or unintended exposure.
The rule aggregates configuration change events and identifies users exceeding a defined threshold within a specified time window. Alert context includes the user, number of changes, time range, affected firewalls, originating systems, and the set of rules modified to support investigation.
While some activity may be legitimate during maintenance windows, unexpected or unapproved bursts of changes should be reviewed for compliance and security impact.

Query:
CommonSecurityLog
| where Activity contains "config"
| where DeviceAction == "edit"
| where isnotempty(DestinationUserName)
// Extract Device Group (optional but useful)
| extend DeviceGroup = extract(@"PanOSFWDeviceGroup=([^;]+)", 1, AdditionalExtensions)
// Normalize identity
| extend 
    Username = DestinationUserName,
    OriginHost = SourceHostName,
    Firewall = DeviceName
// 🔥 Aggregate behavior
| summarize 
    ChangeCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    RuleSet = make_set(Message, 50),
    FirewallSet = make_set(Firewall, 10),
    OriginSet = make_set(OriginHost, 10)
by Username, bin(TimeGenerated, 20m)
// 🔥 Threshold condition
| where ChangeCount >= 5
// Final output
| project 
    TimeGenerated,
    Username,
    ChangeCount,
    FirstSeen,
    LastSeen,
    FirewallSet,
    OriginSet,
    RuleSet
 

MITRE ATT&CK Mapping
🎯 Primary Technique
•	T1562.004 Impair Defenses: Disable or Modify System Firewall 
🎯 Secondary Techniques
•	T1078 Valid Accounts
👉 Legitimate user performing suspicious activity 
•	T1098 Account Manipulation
👉 Potential misuse or persistence through configuration changes 
🎯 MITRE Tactics
•	Defense Evasion 
•	Persistence 
•	Impact (conditional, depending on changes) 

31.	VPN Tunnel Hard Down (IKE Phase-1 Continuous Failure)

Description:
Detects VPN tunnels that continuously attempt IKE Phase-1 negotiation but fail every time without any successful establishment. This indicates a persistent tunnel outage caused by configuration mismatch, connectivity issues, or peer-side problems. The rule identifies tunnels stuck in a failure loop with no recovery.
Query:
CommonSecurityLog
| where Activity == "SYSTEM"
| where DeviceEventClassID == "vpn"
| extend EventSubtype = tolower(extract(@"cat=;([a-z]+(?:-[a-z0-9]+)+)", 1, AdditionalExtensions))
| summarize
    Phase1_Start = countif(EventSubtype == "ike-nego-p1-start"),
    Phase1_Success = countif(EventSubtype == "ike-nego-p1-succ"),
    Phase1_Fail = countif(EventSubtype == "ike-nego-p1-fail"),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by TunnelName = FileName, Firewall = DeviceName, bin(TimeGenerated, 5m)
| where Phase1_Start > 5
  and Phase1_Fail == Phase1_Start
  and Phase1_Success == 0 

MITRE ATT&CK
•	Tactic: Command and Control (TA0011) 
•	Technique: Application Layer Protocol
 32.	VPN Tunnel Flapping (Intermittent IKE Phase-1 Failures)

Description:
Detects VPN tunnels that intermittently fail and succeed during IKE Phase-1 negotiation within a short time window. This behavior indicates unstable connectivity, misconfiguration, or intermittent network issues causing repeated tunnel re-establishment attempts.
Query:
CommonSecurityLog
| where Activity == "SYSTEM"
| where DeviceEventClassID == "vpn"
| extend EventSubtype = tolower(extract(@"cat=;([a-z]+(?:-[a-z0-9]+)+)", 1, AdditionalExtensions))
| summarize
    Phase1_Start = countif(EventSubtype == "ike-nego-p1-start"),
    Phase1_Success = countif(EventSubtype == "ike-nego-p1-succ"),
    Phase1_Fail = countif(EventSubtype == "ike-nego-p1-fail"),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by TunnelName = FileName, Firewall = DeviceName, bin(TimeGenerated, 5m)
| where Phase1_Start >= 3
  and Phase1_Fail > 0
  and Phase1_Success > 0
  and Phase1_Success < Phase1_Start

MITRE ATT&CK
•	Tactic: Command and Control (TA0011) 
•	Technique: Application Layer Protocol


33.	File Decode or Download Followed by Suspicious Activity 

Description:
This analytic rule detects a two-stage attack pattern on Windows endpoints where a file download or decode utility is executed followed by suspicious endpoint activity within a 1-minute window on the same machine. The first stage identifies use of known download utilities such as certutil, bitsadmin, wget, or curl, PowerShell-based file download commands, or file decode operations commonly used to obfuscate malware delivery. If 5 or more suspicious endpoint activities are observed on the same host within 1 minute of the download or decode event, an alert is triggered. This technique is commonly used by attackers to download encoded payloads and decode them locally to evade security controls and antivirus detection.
Query:
let DownloadUtilities = dynamic([
    "certutil", "bitsadmin", "wget", "curl",
    "Invoke-WebRequest", "DownloadFile", "DownloadString",
    "Start-BitsTransfer", "urlcache", "FromBase64String"
]);
let SuspiciousKeywords = dynamic([
    "mimikatz", "procdump", "rundll32", "regsvr32",
    "mshta", "wscript", "cscript", "powershell -enc",
    "powershell -nop", "net user", "net localgroup",
    "whoami", "schtasks", "wmic", "psexec", "nltest"
]);
let AdminServers =
    _GetWatchlist('Admin_Jump_Servers')
    | project ServerName = tolower(tostring(SearchKey));
let AdminAccounts =
    _GetWatchlist('Admin_Service_Accounts')
    | project AccountName = tolower(tostring(SearchKey));
// Hardcoded exemptions based on confirmed investigation
let ExemptComputers = dynamic([
    "AWTSP03.us.ds.amkor.com"
]);
let ExemptUsers = dynamic([
    "aws016239"
]);
let DownloadEvents =
    SecurityEvent
    | where TimeGenerated >= ago(10m)
    | where EventID == 4688
    | where isnotempty(CommandLine)
    | where SubjectUserName !endswith "$"
    | where SubjectUserName !in ("SYSTEM", "LOCAL SERVICE", "NETWORK SERVICE")
    | where tolower(SubjectUserName) !in (ExemptUsers)
    | where tolower(SubjectUserName) !in (AdminAccounts)
    | where Computer !in (ExemptComputers)
    | where tolower(Computer) !in (AdminServers)
    | where CommandLine has_any (DownloadUtilities)
    | project DownloadTime = TimeGenerated, Computer, SubjectUserName,
              DownloadProcess = NewProcessName, DownloadCmdLine = CommandLine;
let SuspiciousEvents =
    SecurityEvent
    | where TimeGenerated >= ago(10m)
    | where EventID == 4688
    | where isnotempty(CommandLine)
    | where SubjectUserName !endswith "$"
    | where SubjectUserName !in ("SYSTEM", "LOCAL SERVICE", "NETWORK SERVICE")
    | where tolower(SubjectUserName) !in (ExemptUsers)
    | where tolower(SubjectUserName) !in (AdminAccounts)
    | where Computer !in (ExemptComputers)
    | where tolower(Computer) !in (AdminServers)
    | where CommandLine has_any (SuspiciousKeywords)
    | project SuspiciousTime = TimeGenerated, Computer, SubjectUserName,
              SuspiciousProcess = NewProcessName, SuspiciousCmdLine = CommandLine;
DownloadEvents
| join kind=inner SuspiciousEvents on Computer
| where SuspiciousTime > DownloadTime
| where SuspiciousTime <= datetime_add('minute', 5, DownloadTime)
| summarize
    TotalSuspiciousEvents   = count(),
    SuspiciousProcesses     = strcat_array(make_set(SuspiciousProcess, 20), ", "),
    SuspiciousCmdLines      = strcat_array(make_set(SuspiciousCmdLine, 20), ", "),
    FirstSeen               = min(DownloadTime),
    LastSeen                = max(SuspiciousTime)
    by Computer, SubjectUserName, DownloadProcess, DownloadCmdLine
| where TotalSuspiciousEvents >= 2
| project
    FirstSeen,
    LastSeen,
    Computer,
    SubjectUserName,
    DownloadProcess,
    DownloadCmdLine,
    TotalSuspiciousEvents,
    SuspiciousProcesses,
    SuspiciousCmdLines
| sort by TotalSuspiciousEvents desc
 

MITRE ATT&CK Mapping
Tactics:
•	Execution
•	Defense Evasion
•	Discovery
•	Collection
Techniques:
•	T1140 – Deobfuscate/Decode Files or Information
•	T1059.001 – Command and Scripting Interpreter: PowerShell
•	T1105 – Ingress Tool Transfer
•	T1027 – Obfuscated Files or Information
•	T1082 – System Information Discovery
•	T1203 – Exploitation for Client Execution

Watchlists to Create:
Watchlist Name	What to Add Now	What to Add Later
Admin_Jump_Servers	AWTSP03.us.ds.amkor.com	Any future confirmed admin servers
Admin_Service_Accounts	aws016239
156368	Any future confirmed admin/service accounts

34.	Palo Alto HIP – High Volume of New Devices Detected

Description:
Detects a surge of previously unseen endpoints connecting through GlobalProtect based on HIPMATCH logs. The rule identifies devices that were not observed in the historical baseline window but appear within a short detection window, indicating potential unauthorized device onboarding, VPN misuse, or large-scale device registration.
Query:
let BaselineWindow = 7d;
let DetectionWindow = 15m;
let Threshold = 20;
// Baseline (historical known devices)
let BaselineDevices =
    CommonSecurityLog
    | where DeviceVendor == "Palo Alto Networks"
    | where Activity == "HIPMATCH"
    | where TimeGenerated between (ago(BaselineWindow) .. ago(DetectionWindow))
    | extend EndpointMAC = extract(@"PanOSEndpointMac=([^;]+)", 1, AdditionalExtensions)
    | extend DeviceId = coalesce(EndpointMAC, SourceIP)
    | where isnotempty(DeviceId)
    | distinct DeviceId;
// Current window (recent devices)
let CurrentDevices =
    CommonSecurityLog
    | where DeviceVendor == "Palo Alto Networks"
    | where Activity == "HIPMATCH"
    | where TimeGenerated > ago(DetectionWindow)
    | extend EndpointMAC = extract(@"PanOSEndpointMac=([^;]+)", 1, AdditionalExtensions)
    | extend OS = coalesce(DeviceCustomString1, DeviceCustomString2, DeviceCustomString3, DeviceCustomString4)
    | extend DeviceId = coalesce(EndpointMAC, SourceIP)
    | where isnotempty(DeviceId);
// New devices with full context
CurrentDevices
| where DeviceId !in (BaselineDevices)
| summarize 
    NewDeviceCount = dcount(DeviceId),
    Devices = make_set(DeviceId),
    IPs = make_set(SourceIP),
    MACs = make_set(EndpointMAC),
    OS_List = make_set(OS),
    Firewalls = make_set(DeviceName),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
| where NewDeviceCount > Threshold
 

MITRE ATT&CK Mapping
Tactics:
•	Initial Access (TA0001) 
•	Persistence (TA0003) 
•	Defense Evasion (TA0005) 
Techniques:
•	Valid Accounts (T1078) 
•	External Remote Services (T1133) 
•	Device Registration Abuse (T1098 – Account Manipulation)

 35.	Anomalous Traffic Spike from Host (Palo Alto Firewall)

Description:
This analytic rule detects abnormal spikes in network traffic originating from a single host based on Palo Alto firewall logs in the CommonSecurityLog table.
The rule establishes a behavioral baseline using a 7-day historical window and calculates the 95th percentile (P95) of traffic volume per source IP. It then compares the current traffic volume (last 10 minutes) against this baseline.
An alert is triggered when:
•	The current traffic exceeds a minimum threshold (10 MB), and 
•	The baseline traffic is significant (>1 MB), and 
•	The current traffic is 3x or more above the historical baseline 
This detection helps identify:
•	Data exfiltration attempts 
•	Malware activity generating abnormal traffic 
•	Internal scanning or lateral movement 
•	Misconfigured applications or unexpected bulk transfers 
The alert includes contextual enrichment such as:
•	Number of destination IPs contacted 
•	Top destination IP 
•	Application protocols used
Query
let TimeWindow = 10m;
let BaselineDays = 7d;
let SpikeMultiplier = 3;
let MinBaselineBytes = 1000000;   // 1 MB
let MinCurrentBytes = 10000000;   // 10 MB
// Current traffic
let CurrentTraffic =
    CommonSecurityLog
    | where TimeGenerated >= ago(TimeWindow)
    | where Activity == "TRAFFIC"
    | where DeviceAction == "allow"
    | extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
    | summarize 
        TotalBytes = sum(TrafficVolume),
        Destinations = dcount(DestinationIP),
        Applications = make_set(ApplicationProtocol, 3),
        TopDestIP = any(DestinationIP)
        by SourceIP;
// Baseline
let BaselineTraffic =
    CommonSecurityLog
    | where TimeGenerated between (ago(BaselineDays) .. ago(TimeWindow))
    | where Activity == "TRAFFIC"
    | where DeviceAction == "allow"
    | extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
    | summarize BaselineP95 = percentile(TrafficVolume, 95) by SourceIP;
// Join + enrich
CurrentTraffic
| join kind=inner BaselineTraffic on SourceIP
| where BaselineP95 > MinBaselineBytes
| where TotalBytes > MinCurrentBytes
| extend SpikeRatio = TotalBytes / BaselineP95
// 🔥 Human readable conversions
| extend 
    CurrentTrafficMB = round(TotalBytes / 1024 / 1024, 2),
    BaselineMB = round(BaselineP95 / 1024 / 1024, 2)
// 🔥 Prioritization
| extend Severity = case(
                        SpikeRatio >= 10,
                        "High",
                        SpikeRatio >= 5,
                        "Medium",
                        "Low"
                    )
// Final output
| project 
    TimeGenerated = now(),
    SourceIP,
    CurrentTrafficMB,
    BaselineMB,
    SpikeRatio,
    Severity,
    Destinations,
    TopDestIP,
    Applications
| order by SpikeRatio desc
 

🎯 MITRE ATT&CK Mapping
🔹 Tactics
•	Exfiltration 
•	Command and Control 
•	Lateral Movement 
________________________________________
🔹 Techniques
•	Exfiltration Over Unencrypted/Obfuscated Non-C2 Protocol 
•	Application Layer Protocol 
•	Remote Services 
•	Data Transfer Size Limits 

 36.	Suspicious C2 Communication with Traffic Spike (Palo Alto Firewall)

Description – 
This analytic rule detects potential Command and Control (C2) activity by correlating abnormal network behavior with threat intelligence signals from Palo Alto firewall logs.
The rule identifies hosts that generate a significant spike in network traffic compared to their historical baseline (7-day P95), and simultaneously trigger threat detections such as:
•	Sinkhole communications 
•	Malicious or newly registered domains (NRD) 
•	Phishing-related URLs 
•	Generic or suspicious domain indicators 
The detection focuses on threat actions such as alert, sinkhole, or block-url, ensuring meaningful security signals.
By combining:
•	Behavioral anomaly (traffic spike) 
•	Threat-based detection (C2 indicators) 
this rule provides high-confidence detection of compromised hosts, including:
•	Malware beaconing to C2 infrastructure 
•	Data exfiltration over suspicious channels 
•	Post-compromise communication activity 
The alert includes enriched context such as:
•	Traffic volume vs baseline 
•	Destination count and top destination 
•	Threat signature, category, and URL

Query:
let TimeWindow = 15m;
let BaselineDays = 7d;
let SpikeMultiplier = 3;
let MinBaselineBytes = 1000000;   // 1 MB
let MinCurrentBytes = 10000000;   // 10 MB
// =========================
// STEP 1: Traffic Spike Detection
// =========================
let CurrentTraffic =
    CommonSecurityLog
    | where TimeGenerated >= ago(TimeWindow)
    | where Activity == "TRAFFIC"
    | where DeviceAction == "allow"
    | extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
    | summarize 
        CurrentBytes = sum(TrafficVolume),
        Destinations = dcount(DestinationIP),
        TopDestIP = any(DestinationIP),
        TrafficTime = max(TimeGenerated)
        by SourceIP;
let BaselineTraffic =
    CommonSecurityLog
    | where TimeGenerated between (ago(BaselineDays) .. ago(TimeWindow))
    | where Activity == "TRAFFIC"
    | where DeviceAction == "allow"
    | extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
    | summarize BaselineP95 = percentile(TrafficVolume, 95) by SourceIP;
let TrafficSpike =
    CurrentTraffic
    | join kind=inner BaselineTraffic on SourceIP
    | where BaselineP95 > MinBaselineBytes
    | where CurrentBytes > MinCurrentBytes
    | extend SpikeRatio = CurrentBytes / BaselineP95
    | where SpikeRatio >= SpikeMultiplier
    | project
        SourceIP,
        CurrentBytes,
        BaselineP95,
        SpikeRatio,
        Destinations,
        TopDestIP,
        TrafficTime;
// =========================
// STEP 2: Threat Detection (Aligned with YOUR data)
// =========================
let ThreatEvents =
    CommonSecurityLog
    | where TimeGenerated >= ago(TimeWindow)
    | where Activity == "THREAT"
    | where DeviceAction in ("alert", "sinkhole", "block-url")
    | where 
        DeviceEventClassID has_any ("generic:", "sinkhole", "Malicious", "phishing", "NRD")
        or DeviceEventCategory has_any ("phishing", "malware", "dns", "tunnel")
    | project 
        SourceIP,
        ThreatTime = TimeGenerated,
        ThreatSignature = DeviceEventClassID,
        ThreatCategory = DeviceEventCategory,
        ThreatAction = DeviceAction,
        ThreatURL = RequestURL,
        ThreatDestIP = DestinationIP;
// =========================
// STEP 3: Correlation (FIXED)
// =========================
TrafficSpike
| join kind=inner ThreatEvents on SourceIP
| where abs(datetime_diff("minute", TrafficTime, ThreatTime)) <= 15
// =========================
// STEP 4: Enrichment
// =========================
| extend 
    CurrentTrafficMB = round(CurrentBytes / 1024 / 1024, 2),
    BaselineMB = round(BaselineP95 / 1024 / 1024, 2),
    Severity = case(
               SpikeRatio >= 10,
               "High",
               SpikeRatio >= 5,
               "Medium",
               "Low"
           )
| project 
    TimeGenerated = TrafficTime,
    SourceIP,
    CurrentTrafficMB,
    BaselineMB,
    SpikeRatio,
    Severity,
    Destinations,
    TopDestIP,
    ThreatSignature,
    ThreatCategory,
    ThreatAction,
    ThreatURL,
    ThreatDestIP
| order by SpikeRatio desc
 

🎯 MITRE ATT&CK Mapping
________________________________________
🔹 Tactics
•	Command and Control 
•	Exfiltration 
•	Execution 
________________________________________
🔹 Techniques
•	Application Layer Protocol
→ C2 communication over HTTP/HTTPS/SSL 
•	Ingress Tool Transfer
→ Payload download / data exchange 
•	Exfiltration Over C2 Channel
→ Data exfiltration via same channel 
•	Domain Generation Algorithms
→ DGA / suspicious domain behavior 
•	Encrypted Channel
→ SSL/TLS-based communication

 37.	VPN Tunnel Failure with Concurrent Network Traffic (Potential VPN Bypass / Misuse)

Description – 
This analytic rule detects scenarios where a VPN tunnel fails to establish successfully, yet network traffic is still observed through the firewall during the same time window.
The rule analyzes Palo Alto SYSTEM logs to identify VPN Phase 1 negotiation failures (ike-nego-p1-fail) with no successful establishment (ike-nego-p1-succ). It then correlates these events with TRAFFIC logs to determine whether network activity continues despite the failed VPN connection.
This behavior may indicate:
•	VPN bypass or split tunneling 
•	Misconfigured network or security policies 
•	Unauthorized access paths into the network 
•	Direct internet communication instead of enforced VPN routing 
•	Potential data exfiltration outside controlled tunnels 
The rule prioritizes alerts based on traffic volume and failure frequency, highlighting high-risk scenarios such as large data transfers occurring without a valid VPN tunnel.
Query –
let TimeWindow = 30m;
let MinTrafficBytes = 1000000; // 1 MB threshold
// =========================
// STEP 1: SYSTEM VPN Failures (Your Logic)
// =========================
let VPNFailures =
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity contains "system"
| where DeviceEventClassID == "vpn"
| extend EventSubtype = tolower(extract(@"cat=;([a-z]+(?:-[a-z0-9]+)+)", 1, AdditionalExtensions))
| summarize
    Phase1_Start = countif(EventSubtype == "ike-nego-p1-start"),
    Phase1_Success = countif(EventSubtype == "ike-nego-p1-succ"),
    Phase1_Fail = countif(EventSubtype == "ike-nego-p1-fail"),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by TunnelName = FileName, Firewall = DeviceName, TimeBin = bin(TimeGenerated, 5m)
| where Phase1_Fail > 0
| where Phase1_Success == 0;
// =========================
// STEP 2: TRAFFIC Activity
// =========================
let TrafficActivity =
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where DeviceAction == "allow"
| extend TrafficVolume = coalesce(SentBytes,0) + coalesce(ReceivedBytes,0)
| summarize
    TotalBytes = sum(TrafficVolume),
    ConnectionCount = count(),
    SourceIPs = make_set(SourceIP, 5),
    DestIPs = make_set(DestinationIP, 5),
    Applications = make_set(ApplicationProtocol, 3)
by Firewall = DeviceName, TimeBin = bin(TimeGenerated, 5m)
| where TotalBytes > MinTrafficBytes;
// =========================
// STEP 3: Correlation
// =========================
VPNFailures
| join kind=inner TrafficActivity on Firewall, TimeBin
// =========================
// STEP 4: Enrichment (FIXED)
// =========================
| extend TrafficMB = round(TotalBytes / 1024 / 1024, 2)
| extend Severity = case(
        TrafficMB > 5000, "Critical",
        TrafficMB > 1000, "High", 
        Phase1_Fail >= 10 and TrafficMB > 100, "High",
        Phase1_Fail >= 5, "Medium",
        "Low"
    )
| project
    TimeGenerated = TimeBin,
    Firewall,
    TunnelName,
    Phase1_Start,
    Phase1_Fail,
    Phase1_Success,
    TrafficMB,
    ConnectionCount,
    SourceIPs,
    DestIPs,
    Applications,
    Severity
| order by TrafficMB desc

38.	Malware URL Access – Allowed or Alerted Traffic

Description –
Detects when an internal host attempts to access URLs categorized as malware, where the traffic was allowed or only alerted by the firewall. This indicates a potential compromise, command-and-control communication, or failure in enforcement policy.
Query –
CommonSecurityLog
| where Activity == "THREAT"
| where DeviceEventClassID == "url"
// Extract
| extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
| extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
// Split
| extend URLCategoryArray = split(URLCategoryRaw, ",")
| mv-expand URLCategoryArray
| extend URLCategory = trim(@" """, tostring(URLCategoryArray))
// Filter malware
| where URLCategory contains "malware"
// Allowed traffic only
| where DeviceAction has_any ("allow", "alert")
// Output
| project 
    TimeGenerated,
    DeviceName,
    SourceIP,
    DestinationIP,
    RequestURL,
    DeviceAction,
    ApplicationProtocol,
    URLCategory,
    SourceUserName
		
🧬 MITRE ATT&CK Mapping
•	T1071 – Application Layer Protocol 
•	T1105 – Ingress Tool Transfer 
•	T1041 – Exfiltration Over C2 Channel 
•	T1566 – Phishing

39.	Use Case: High-Risk URL Access (Allowed/Alerted)

Description –
Detects when an internal host accesses URLs categorized as high-risk by the Palo Alto firewall, where the traffic is allowed or only alerted instead of being blocked. This indicates potential exposure to malicious or suspicious content and may suggest policy gaps, user risk behavior, or early-stage compromise.
Query –
CommonSecurityLog
| where Activity contains "THREAT"
| where DeviceEventClassID == "url"
// --- Extract ---
| extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
| extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
// --- Split & Expand ---
| extend URLCategoryArray = split(URLCategoryRaw, ",")
| mv-expand URLCategoryArray
| extend CategoryItem = trim(@" """, tostring(URLCategoryArray))
// --- Filter High-Risk ---
| where CategoryItem has "high-risk"
// --- Allowed / Alerted Traffic ---
| where DeviceAction has_any ("allow", "alert")
// --- Output ---
| project 
    TimeGenerated,
    DeviceName,
    SourceIP,
    DestinationIP,
    RequestURL,
    DeviceAction,
    ApplicationProtocol,
    URLRisk = CategoryItem,
    DeviceAction,
    ApplicationProtocol,
    URLCategory,
    SourceUserName

MITRE ATT&CK Mappings
•	T1071 – Application Layer Protocol
(Use of web protocols for malicious communication) 
•	T1566 – Phishing
(User accessing malicious or deceptive web content) 
•	T1105 – Ingress Tool Transfer
(Downloading malicious payloads from high-risk domains) 
•	T1204 – User Execution
(User interaction leading to compromise) 
•	T1041 – Exfiltration Over C2 Channel (conditional)
(If high-risk domains are used for data exfiltration)

40.	Repeated Blocked High-Risk URL Attempts from Same Source

Description –
Detects when a single host generates multiple blocked requests to high-risk URLs within a short time window. This behavior may indicate an infected endpoint repeatedly attempting to communicate with malicious infrastructure, automated scripts, or persistent attempts to access dangerous web content despite enforcement controls.
Query –
CommonSecurityLog
| where TimeGenerated >= ago(1h)
| where Activity contains "THREAT"
| where DeviceEventClassID == "url"
// --- Extract ---
| extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
| extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
// --- Split & Expand ---
| extend URLCategoryArray = split(URLCategoryRaw, ",")
| mv-expand URLCategoryArray
| extend CategoryItem = trim(@" """, tostring(URLCategoryArray))
// --- Filter High-Risk ---
| where CategoryItem has "high-risk"
// --- Blocked Traffic Only ---
| where DeviceAction has "block"
// --- Aggregate ---
| summarize 
    AttemptCount = count(),
    URLs = make_set(RequestURL, 50),
    DestIPs = make_set(DestinationIP, 50),
    Applications = make_set(ApplicationProtocol, 20),
    Users = make_set(SourceUserName, 20)
by 
    SourceIP,
    DeviceName,
    bin(TimeGenerated, 5m)
// --- Threshold ---
| where AttemptCount > 5
// --- Output ---
| project 
    TimeGenerated,
    DeviceName,
    SourceIP,
    Users = strcat_array(Users, ", "),
    AttemptCount,
    URLs = strcat_array(URLs, ", "),
    DestIPs = strcat_array(DestIPs, ", "),
    Applications = strcat_array(Applications, ", ")
🧬 MITRE ATT&CK Mappings
•	T1071 – Application Layer Protocol
(Use of web protocols for command-and-control communication) 
•	T1105 – Ingress Tool Transfer
(Attempts to download payloads from high-risk domains) 
•	T1041 – Exfiltration Over C2 Channel
(Repeated outbound attempts to suspicious infrastructure) 
•	T1204 – User Execution
(User-triggered activity leading to blocked malicious access) 
•	T1566 – Phishing (contextual)
(Access attempts to high-risk domains originating from phishing links)

41.	High-Risk URL Allowed via Whitelist or Allow Policy

Description –
Detects when URLs categorized as high-risk are accessed and allowed by firewall policies, particularly those intended for whitelisting or business exceptions (e.g., ati-whitelist, atk_url_allowy, or other allow rules). This behavior indicates potential misuse of allow policies, security control bypass, or exposure to malicious infrastructure that was not properly blocked.
Query –
CommonSecurityLog
| where Activity contains "THREAT"
| where DeviceEventClassID == "url"
// --- Extract Category ---
| extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
| extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
// --- Split & Expand ---
| extend URLCategoryArray = split(URLCategoryRaw, ",")
| mv-expand URLCategoryArray
| extend CategoryItem = trim(@" """, tostring(URLCategoryArray))
// --- Separate ---
| extend 
    URLRisk = iff(CategoryItem has "risk", CategoryItem, ""),
    URLCategory = iff(CategoryItem !has "risk", CategoryItem, "")
// --- Filter High-Risk ---
| where URLRisk == "high-risk"
// --- Allowed / Alerted ---
| where DeviceAction has_any ("allow", "alert")
// --- Policy Rule Filter (IMPORTANT: adjust field if needed) ---
| where DeviceCustomString3 has_any ("ati-whitelist", "atk_url_allowy", "whitelist", "allow")
// --- Output ---
| project 
    TimeGenerated,
    DeviceName,
    SourceIP,
    DestinationIP,
    RequestURL,
    URLCategory,
    URLRisk,
    DeviceAction,
    ApplicationProtocol,
    PolicyRule = DeviceCustomString3,
    SourceUserName

🧬 MITRE ATT&CK Mappings
•	T1071 – Application Layer Protocol
(Use of web protocols for communication with potentially malicious infrastructure) 
•	T1105 – Ingress Tool Transfer
(Downloading malicious payloads from high-risk domains) 
•	T1566 – Phishing
(User access to malicious or deceptive websites) 
•	T1204 – User Execution
(User-driven interaction leading to risky web access) 
•	T1041 – Exfiltration Over C2 Channel (contextual)
(Allowed communication to suspicious or attacker-controlled domains)

42.	Palo Alto – Spyware/Virus Activity Allowed (Policy Bypass)

Description –
Detects instances where the Palo Alto firewall identifies spyware, virus, or backdoor activity but the traffic is allowed (alert action) instead of being blocked.
This indicates a security control gap or misconfigured policy, where known malicious traffic is permitted into or out of the network.
These alerts are high fidelity and often indicate:
•	Malware execution or propagation 
•	Inadequate threat prevention policy 
•	Exceptions allowing malicious traffic
Query –
CommonSecurityLog
| where Activity contains "THREAT"
// --- Strict malware ---
| where DeviceEventCategory has_any ("spyware", "virus")
// --- Allowed traffic ---
| where DeviceAction == "alert"
// --- Internal host ---
| where ipv4_is_private(SourceIP)
// --- Parse PAN fields ---
| extend PanThreatCategory = extract(@"PanOSThreatCategory=([^;]+)", 1, AdditionalExtensions)
// --- Normalize --
| extend ThreatName = tostring(DeviceEventClassID)
// --- Aggregate ---
| summarize 
    EventCount = count(),
    ThreatNames = make_set(ThreatName, 20),
    ThreatTypes = make_set(PanThreatCategory, 20)
    by SourceIP, DestinationIP, bin(TimeGenerated, 5m), DeviceEventCategory
| where EventCount >= 5

✅ MITRE ATT&CK Mapping
•	🎯 Tactic: Execution
o	T1204 – User Execution 
•	🎯 Tactic: Command and Control
o	T1071 – Application Layer Protocol 
•	🎯 Tactic: Defense Evasion
o	T1562 – Impair Defenses

43.	Palo Alto – Suspicious DNS Communication to Malicious Domains (C2 Activity)

Description –
Detects internal hosts making DNS requests to domains associated with:
•	Command & Control (C2) 
•	Malware infrastructure 
•	Suspicious or malicious categories 
This behavior is commonly associated with:
•	Malware beaconing 
•	Data exfiltration staging 
•	Active compromise 
Even if blocked, repeated attempts indicate an infected host.
Query –
CommonSecurityLog
| where Activity contains "THREAT"
// --- DNS only ---
| where ApplicationProtocol == "dns-base"
// --- Parse PAN threat ---
| extend PanThreatCategory = extract(@"PanOSThreatCategory=([^;]+)", 1, AdditionalExtensions)
// --- Focus on C2 / malicious ---
| where PanThreatCategory has_any ("c2", "malware", "spyware", "phishing", "dns")
// --- Extract domain ---
| extend Domain = extract(@"([a-zA-Z0-9.-]+\.[a-zA-Z]{2,})", 1, Message)
// --- Filter valid ---
| where isnotempty(Domain)
// --- Internal host ---
| where ipv4_is_private(SourceIP)
// --- Aggregate ---
| summarize 
    EventCount = count(),
    Domains = make_set(Domain, 50),
    ThreatTypes = make_set(PanThreatCategory, 20)
    by SourceIP, bin(TimeGenerated, 5m)
| where EventCount >= 3



✅ MITRE ATT&CK Mapping
•	🎯 Tactic: Command and Control
o	T1071.004 – DNS 
•	🎯 Tactic: Exfiltration
o	T1048 – Exfiltration Over Alternative Protocol 
•	🎯 Tactic: Persistence (Optional context)
o	T1105 – Ingress Tool Transfer

44.	Local L2L SNMP Scanner

Description
This analytic rule detects potential internal SNMP-based reconnaissance activity where a single internal source host communicates with more than 59 unique internal destination IP addresses on common SNMP ports within a 10-minute window. The query analyzes firewall and network logs in the CommonSecurityLog table and focuses exclusively on LAN to LAN traffic where both source and destination are private IP addresses. Known vulnerability assessment scanners and authorized scanning IPs are excluded via watchlists to suppress legitimate scanning activity. This behavior is characteristic of internal network discovery tools, compromised hosts attempting to enumerate network devices, or attackers mapping SNMP-enabled infrastructure such as routers, switches, and printers prior to lateral movement or exploitation.
Query:
let SNMPPorts = dynamic([161, 162, 10161, 10162]);
let VA_Scanners =
    _GetWatchlist('VA_Scanners')
    | project ExcludedIP = tostring(SearchKey);
let ValidScanners =
    _GetWatchlist('Valid_Scanning_IP')
    | project ExcludedIP = tostring(SearchKey);
let ExcludedIPs =
    union VA_Scanners, ValidScanners
    | distinct ExcludedIP;
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
| where isnotempty(SourceIP)
| where isnotempty(DestinationIP)
| where DestinationPort in (SNMPPorts)
| where SourceIP !in (ExcludedIPs)
| summarize
    hint.shufflekey = SourceIP
    EventCount          = count(),
    UniqueDestIPs       = dcount(DestinationIP),
    DestIPs             = strcat_array(make_set(DestinationIP, 100), ", "),
    DestPorts           = strcat_array(make_set(DestinationPort, 10), ", "),
    Protocols           = strcat_array(make_set(Protocol, 5), ", "),
    Applications        = strcat_array(make_set(ApplicationProtocol, 5), ", "),
    Actions             = strcat_array(make_set(DeviceAction, 5), ", "),
    Vendors             = strcat_array(make_set(DeviceVendor, 5), ", "),
    Products            = strcat_array(make_set(DeviceProduct, 5), ", "),
    StartTime           = min(TimeGenerated),
    EndTime             = max(TimeGenerated)
    by SourceIP, bin(TimeGenerated, 10m)
| where EventCount > 5
| where UniqueDestIPs > 59
| project
    StartTime,
    EndTime,
    SourceIP,
    EventCount          = tostring(EventCount),
    UniqueDestIPs       = tostring(UniqueDestIPs),
    DestPorts,
    Protocols,
    Applications,
    Actions,
    Vendors,
    Products,
    DestIPs
| sort by UniqueDestIPs desc
 

Watchlists Required
Watchlist Name	Contents	Example Entries
VA_Scanners	Vulnerability assessment scanner IPs	Nessus, Qualys scanner IPs
Valid_Scanning_IP	Authorized internal scanning tool IPs	Approved pen test IPs
________________________________________
MITRE ATT&CK Mapping
Tactics:
•	Discovery
•	Reconnaissance
Techniques:
•	T1046 – Network Service Discovery
•	T1018 – Remote System Discovery
•	T1592 – Gather Victim Host Information
•	T1590 – Gather Victim Network Information


 45.	New Service Discovered in DMZ

Description
This analytic rule detects when a new network service or port is discovered on an existing host within the DMZ network segment. The rule monitors for new port discovery events on hosts that have been known in the environment for more than 24 hours, meaning the host is not new but has a newly opened or exposed service. This is significant in DMZ environments because any new service appearing on a DMZ host could indicate unauthorized software installation, backdoor deployment, misconfiguration, or a compromised host opening new listening ports. The rule fires a maximum of once per hour per host to prevent alert flooding during active scanning windows.
Query:
let DMZSubnets = dynamic([
    "10.x.x.x",     // Replace with your actual DMZ subnet ranges
    "172.x.x.x",    // Replace with your actual DMZ subnet ranges
    "192.168.x.x"   // Replace with your actual DMZ subnet ranges
]);
let DMZHosts =
    _GetWatchlist('DMZ_Hosts')
    | project DMZHost = tostring(SearchKey);
let KnownPorts =
    _GetWatchlist('DMZ_Known_Ports')
    | project
        HostIP      = tostring(column_ifexists("HostIP", "")),
        KnownPort   = toint(column_ifexists("Port", "0"));
CommonSecurityLog
| where TimeGenerated >= ago(1h)
// Scope to DMZ hosts only
| where SourceIP in (DMZHosts)
      or DestinationIP in (DMZHosts)
// Focus on new port or service discovery events
| where Activity has_any (
    "new port", "new service", "port discovered",
    "service discovered", "new connection", "new session"
  )
  or DeviceEventClassID has_any (
    "new-port", "new-service", "port-scan", "host-discovery"
  )
// Host must be known for more than 24 hours — not a new host
| where TimeGenerated >= ago(1h)
// Only established/allowed connections — not blocked scans
| where DeviceAction !in ("deny", "drop", "block", "reject")
| where isnotempty(DestinationPort)
// Exclude well known always-open ports to reduce noise
| where DestinationPort !in (80, 443, 22, 21, 25, 53)
// Anti-join against known ports watchlist
| join kind=leftanti KnownPorts
    on $left.DestinationIP == $right.HostIP
    and $left.DestinationPort == $right.KnownPort
| summarize
    hint.shufflekey = DestinationIP
    EventCount          = count(),
    NewPorts            = strcat_array(make_set(DestinationPort, 20), ", "),
    Protocols           = strcat_array(make_set(Protocol, 5), ", "),
    Applications        = strcat_array(make_set(ApplicationProtocol, 10), ", "),
    SourceIPs           = strcat_array(make_set(SourceIP, 20), ", "),
    Actions             = strcat_array(make_set(DeviceAction, 5), ", "),
    Vendors             = strcat_array(make_set(DeviceVendor, 5), ", "),
    Products            = strcat_array(make_set(DeviceProduct, 5), ", "),
    Activities          = strcat_array(make_set(Activity, 10), ", "),
    StartTime           = min(TimeGenerated),
    EndTime             = max(TimeGenerated)
    by DestinationIP, bin(TimeGenerated, 1h)
| where EventCount > 0
| project
    StartTime,
    EndTime,
    DestinationIP,
    EventCount          = tostring(EventCount),
    NewPorts,
    Protocols,
    Applications,
    SourceIPs,
    Actions,
    Vendors,
    Products,
    Activities
| sort by EventCount desc

Watchlists Required
Watchlist Name	Contents	Example Entries
DMZ_Hosts	All known DMZ host IPs	10.10.50.1, 10.10.50.2
DMZ_Known_Ports	Known legitimate ports per DMZ host	HostIP: 10.10.50.1, Port: 443
________________________________________
Important Note on DMZ Subnet Configuration
The query uses a DMZ_Hosts watchlist as the primary scoping mechanism. Before deploying, replace the placeholder subnet ranges with your actual DMZ ranges or populate the watchlist with confirmed DMZ host IPs. This is critical — without correct DMZ scoping the rule will either miss events or fire on non-DMZ hosts.
________________________________________
MITRE ATT&CK Mapping
Tactics:
•	Discovery
•	Persistence
•	Command and Control
Techniques:
•	T1046 – Network Service Discovery
•	T1205 – Traffic Signaling
•	T1571 – Non-Standard Port
•	T1543 – Create or Modify System Process
•	T1190 – Exploit Public Facing Application


 46.	Database Remote Login Success

Description
This analytic rule detects successful remote authentication events targeting internal database servers. The rule monitors Windows Security Event logs for successful logon events where the source is a remote network address and the destination host is a known database server defined in the database servers watchlist. Unauthorized or unexpected remote logins to database servers may indicate credential compromise, lateral movement, or an attacker attempting to access sensitive data stores directly. This rule helps ensure that only approved users and systems are authenticating to critical database infrastructure from remote locations.
Query:
let DatabaseServers =
    _GetWatchlist('Database_Servers')
    | project ServerName = tolower(tostring(SearchKey));
SecurityEvent
| where TimeGenerated >= ago(5m)
| where EventID == 4624
| where LogonType in (3, 10)
| where isnotempty(IpAddress)
| where IpAddress !in ("127.0.0.1", "::1", "-", "0.0.0.0")
| where tolower(Computer) in (DatabaseServers)
| where TargetUserName !endswith "$"
| where TargetUserName !in ("SYSTEM", "LOCAL SERVICE", "NETWORK SERVICE", "ANONYMOUS LOGON")
| project
    TimeGenerated,
    Computer,
    TargetUserName,
    TargetDomainName,
    SubjectUserName,
    SubjectDomainName,
    LogonType,
    LogonTypeName,
    LogonProcessName,
    AuthenticationPackageName,
    IpAddress,
    IpPort,
    WorkstationName,
    LogonGuid,
    TargetLogonId,
    SubjectLogonId
| sort by TimeGenerated desc 

Watchlists Required
Watchlist Name	Contents	Example Entries
Database_Servers	Hostnames or IPs of all database servers	SQLPROD01, ORADB02, 10.0.5.20
________________________________________
MITRE ATT&CK Mapping
Tactics:
•	Initial Access
•	Lateral Movement
•	Collection
Techniques:
•	T1078 – Valid Accounts
•	T1021.001 – Remote Services: Remote Desktop Protocol
•	T1021.002 – Remote Services: SMB Windows Admin Shares
•	T1213 – Data from Information Repositories
•	T1190 – Exploit Public Facing Application


47.	Local L2L DNS Scanner 
Description
This analytic rule detects potential internal DNS-based reconnaissance activity where a single internal source host sends traffic to more than 59 unique internal destination IP addresses on common DNS ports within a 10-minute window. The query analyzes firewall and network logs in the CommonSecurityLog table and focuses exclusively on LAN to LAN traffic where both source and destination are private IP addresses. Known DNS servers, vulnerability assessment scanners, authorized scanning IPs, and Domain Controllers are excluded via watchlists to suppress legitimate high-volume DNS traffic. This behavior is indicative of automated internal network discovery, malware performing lateral movement preparation, or a compromised host mapping internal network topology via DNS.
Query:
let DNSPorts = dynamic([53, 5353, 5355]);
let ExcludedIPs =
    union
        (_GetWatchlist('DNS_Servers') | project ExcludedIP = tostring(SearchKey)),
        (_GetWatchlist('VA_Scanners') | project ExcludedIP = tostring(SearchKey)),
        (_GetWatchlist('Valid_Scanning_IP') | project ExcludedIP = tostring(SearchKey)),
        (_GetWatchlist('DomainControllers_IP') | project ExcludedIP = tostring(SearchKey))
    | distinct ExcludedIP;
CommonSecurityLog
| where TimeGenerated >= ago(10m)
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
| where isnotempty(SourceIP)
| where isnotempty(DestinationIP)
| where DestinationPort in (DNSPorts)
| where SourceIP !in (ExcludedIPs)
| where DestinationIP !in (ExcludedIPs)
| summarize
    hint.shufflekey = SourceIP
    EventCount          = count(),
    UniqueDestIPs       = dcount(DestinationIP),
    DestIPs             = strcat_array(make_set(DestinationIP, 100), ", "),
    DestPorts           = strcat_array(make_set(DestinationPort, 10), ", "),
    Protocols           = strcat_array(make_set(Protocol, 5), ", "),
    Applications        = strcat_array(make_set(ApplicationProtocol, 5), ", "),
    Actions             = strcat_array(make_set(DeviceAction, 5), ", "),
    Vendors             = strcat_array(make_set(DeviceVendor, 5), ", "),
    Products            = strcat_array(make_set(DeviceProduct, 5), ", "),
    StartTime           = min(TimeGenerated),
    EndTime             = max(TimeGenerated)
    by SourceIP, bin(TimeGenerated, 10m)
| where EventCount > 5
| where UniqueDestIPs > 59
| project
    StartTime,
    EndTime,
    SourceIP,
    EventCount          = tostring(EventCount),
    UniqueDestIPs       = tostring(UniqueDestIPs),
    DestPorts,
    Protocols,
    Applications,
    Actions,
    Vendors,
    Products,
    DestIPs
| sort by UniqueDestIPs desc
 
Watchlists Required
Watchlist Name	Contents	Example Entries
DNS_Servers	All internal DNS server IPs	10.0.0.53, 10.0.1.53
VA_Scanners	Vulnerability assessment scanner IPs	Nessus, Qualys scanner IPs
Valid_Scanning_IP	Authorized internal scanning tool IPs	Approved pen test IPs
DomainControllers_IP	All internal Domain Controller IPs	10.0.0.1, 10.0.0.2
________________________________________
MITRE ATT&CK Mapping
Tactics:
•	Discovery
•	Reconnaissance
Techniques:
•	T1046 – Network Service Discovery
•	T1018 – Remote System Discovery
•	T1590.002 – Gather Victim Network Information: DNS
•	T1071.004 – Application Layer Protocol: DNS







48.	Suspicious Database Access Pattern (Denied Attempts Followed by Successful Connection)
Name in sentinel – PaloAlto - Multiple database access failure followed by a success
Description:
This analytic rule detects suspicious database access behavior where a source IP generates one or more denied database connection attempts followed by at least one successful connection within a 20-minute time window.
The detection leverages Palo Alto App-ID to accurately identify database protocols (e.g., MSSQL, Oracle, MySQL), ensuring high-fidelity monitoring of database-related traffic.
Such activity may indicate:
•	Unauthorized access attempts followed by eventual success 
•	Credential guessing or weak authentication controls 
•	Misconfigured applications retrying connections 
•	Early-stage lateral movement targeting database systems 
The rule correlates network-level deny and allow actions from the same source IP against database services to highlight potentially risky access patterns.
Query
let dbProtocols = dynamic([
    "mssql-db-encrypted",
    "mssql-db-unencrypted",
    "mssql-db-base",
    "mssql-mon",
    "mysql",
    "oracle",
    "oracle-eloqua",
    "oracle-forms"
]);
CommonSecurityLog
// | where TimeGenerated >= ago(1h)   // Adjust based on rule schedule
| where Activity contains "TRAFFIC"
| where ApplicationProtocol in (dbProtocols)
// --- Normalize action ---
| extend Action = tolower(DeviceAction)
// --- Focus only on allow/deny ---
| where Action in ("allow", "deny")
| summarize
    AllowCount = countif(Action in ("allow")),
    DenyCount = countif(Action == "deny"),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    TargetIPs = make_set(DestinationIP, 10),
    Protocols = make_set(ApplicationProtocol, 5)
by SourceIP, bin(TimeGenerated, 20m)
// --- Apply thresholds ---
| where DenyCount >= 1 and AllowCount > 0
// --- Output ---
| project
    TimeGenerated,
    SourceIP,
    DenyCount,
    AllowCount,
    FirstSeen,
    LastSeen,
    TargetIPs,
    Protocols
| order by DenyCount desc
 
 

MITRE ATT&CK Mapping
🔹 Primary Technique
•	T1110
Rationale: Multiple denied attempts suggest repeated access attempts that may indicate password guessing or authentication abuse. 
🔹 Secondary Techniques
•	T1078
Rationale: The eventual successful connection may indicate compromised or discovered valid credentials. 
•	T1021
Rationale: Database services (MSSQL, Oracle, MySQL) are commonly used for remote access and lateral movement. 
🔹 Optional / Contextual
•	T1046
Rationale: Initial denied attempts may represent probing of available database services.

49.	Excessive Firewall Denies from Internal Host (Potential Lateral Movement or Internal Scanning)

Description
This analytic rule detects suspicious behavior originating from internal hosts that generate a high volume of denied firewall connection attempts within a short time window.
The rule analyzes firewall logs over a 5-minute period and identifies internal source IP addresses that exceed 100 denied connection attempts while targeting more than 10 unique destination ports. This pattern is indicative of scanning or probing activity originating from within the network.
Such behavior may suggest:
•	Internal reconnaissance or port scanning 
•	Lateral movement attempts across internal systems 
•	Malware propagation performing automated scanning 
•	Misconfigured applications repeatedly attempting connections 
The rule enriches alerts with contextual information such as destination IPs, ports, hostnames, protocols, and device details to support rapid investigation and triage.
Query
CommonSecurityLog
| where TimeGenerated >= ago(5m)
| where DeviceAction == "deny"
// --- Focus on LOCAL (internal) sources ---
| where ipv4_is_private(SourceIP)
// --- Aggregation ---
| summarize
    DenyCount = count(),
    UniquePorts = dcount(DestinationPort),
    Ports = strcat_array(make_set(DestinationPort, 10), ", "),
    DestinationIPs = strcat_array(make_set(DestinationIP, 10), ", "),
    DestinationHosts = strcat_array(make_set(DestinationHostName, 10), ", "),
    Protocols = strcat_array(make_set(Protocol, 10), ", "),
    DeviceProducts = strcat_array(make_set(DeviceProduct, 5), ", "),
    DeviceVendors = strcat_array(make_set(DeviceVendor, 5), ", ")
by SourceIP
// --- Thresholds ---
| where DenyCount > 100
| where UniquePorts > 10
| order by DenyCount desc

🎯 MITRE ATT&CK Mapping
🔹 Primary Technique
•	T1046
Internal host scanning multiple ports/services to identify accessible systems. 
🔹 Secondary Techniques
•	T1021
Attempts to connect to multiple services may indicate lateral movement. 
•	T1018
Broad probing across internal hosts to enumerate reachable systems. 
🔹 Optional / Contextual Techniques
•	T1105
If scanning precedes payload delivery. 
•	T1059
If activity is driven by scripts or automated tooling.








50.	Suspicious Distributed DNS Activity from Internal Host (Potential C2 or DNS Tunneling)

Description:
This analytic rule detects anomalous DNS behavior originating from internal hosts that communicate with a large number of uncommon external DNS destinations.
The rule analyzes DNS traffic (dns-base) over a one-hour period and dynamically excludes the most frequently contacted DNS servers in the environment to reduce noise from legitimate resolvers. It then identifies internal source IPs that generate a high volume of DNS requests distributed across many unique external IP addresses.
Such behavior deviates from normal DNS patterns, where clients typically communicate with a limited set of resolvers, and may indicate:
•	Command-and-control (C2) communication over DNS 
•	Domain Generation Algorithm (DGA) activity 
•	DNS tunneling or covert data exfiltration 
•	Malware beaconing to distributed infrastructure 
The alert triggers when a host exceeds defined thresholds for both total DNS requests and diversity of destination IPs, highlighting potentially compromised systems using DNS as a communication channel.
Query:
let CommonDNS =
    CommonSecurityLog
    | where TimeGenerated >= ago(24h)
    | where Activity == "TRAFFIC"
    | where ApplicationProtocol == "dns-base"
    | summarize Count = count() by DestinationIP
    | top 20 by Count
    | project DestinationIP;
CommonSecurityLog
| where TimeGenerated >= ago(1h)
| where Activity == "TRAFFIC"
| where ApplicationProtocol == "dns-base"
// --- Local to Remote ---
| where ipv4_is_private(SourceIP)
| where not(ipv4_is_private(DestinationIP))
// --- Remove common DNS servers ---
| where DestinationIP !in (CommonDNS)
// --- Aggregate ---
| summarize
    DNSRequestCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 20),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP
// --- Stronger detection ---
| where DNSRequestCount > 50
| where UniqueDestinations > 20
| order by UniqueDestinations desc
 


🎯 MITRE ATT&CK Mappings
•	T1071.004 
•	T1568 
•	T1048 
•	T1105

51.	Abnormal Encrypted DNS Usage from Internal Host (Potential DNS Evasion or C2 Communication)

Description – 
This analytic rule detects anomalous usage of encrypted DNS protocols—specifically DNS over HTTPS (DoH) and DNS over TLS (DoT)—originating from internal hosts.
The rule analyzes firewall traffic logs to identify internal source IPs generating a high volume of encrypted DNS requests to multiple external destinations within a defined time window. While encrypted DNS is commonly used for privacy by modern applications and browsers, excessive or distributed usage may indicate attempts to bypass traditional DNS monitoring and security controls.
Such behavior can be associated with:
•	Malware leveraging encrypted DNS for command-and-control (C2) communication 
•	Evasion of network security controls and DNS inspection mechanisms 
•	Unauthorized or suspicious applications using external DNS resolvers 
•	Covert data exfiltration over encrypted channels 
The detection highlights hosts that deviate from typical DNS behavior, enabling security teams to investigate potential compromise or misuse.
Query
CommonSecurityLog
| where Activity == "TRAFFIC"
| where ApplicationProtocol in ("dns-over-https", "dns-over-tls")
// --- Internal hosts only ---
| where ipv4_is_private(SourceIP)
// --- Aggregate ---
| summarize
    RequestCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 15),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, ApplicationProtocol
// --- Detection logic ---
| where RequestCount > 100
| where UniqueDestinations > 5
| order by RequestCount desc
 

🎯 MITRE ATT&CK Mappings
•	T1071.004 
•	T1573 
•	T1048

52.	Potential Botnet Communication from Internal Host

Description - 
This analytic rule detects internal hosts communicating with destinations flagged as Botnet-related by threat intelligence. It focuses on traffic where connections are allowed or alerted by the firewall and aggregates activity over time to identify patterns of repeated communication with multiple external IP addresses.
The rule highlights internal source IPs that generate multiple botnet-associated events, including connections to several unique destinations and ports, which may indicate potential command-and-control (C2) communication, beaconing behavior, or early-stage compromise.
Query -
CommonSecurityLog
// | where TimeGenerated >= ago(24h)
| where isnotempty(IndicatorThreatType)
| where IndicatorThreatType has "Botnet"
// --- Focus on meaningful actions ---
| where tolower(DeviceAction) in ("alert", "allow", "allowed")
| where ipv4_is_private(SourceIP)
// --- Aggregate ---
| summarize
    BotnetEventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    DestinationIPs = make_set(DestinationIP, 10),
    DestinationPorts = make_set(DestinationPort, 10),
    Protocols = make_set(Protocol, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, IndicatorThreatType
// --- Tune threshold ---
| where BotnetEventCount >= 5
| order by BotnetEventCount desc
| take 10

🎯 MITRE ATT&CK Mapping
Tactic: Command and Control (TA0011)
•	Application Layer Protocol (T1071) 
•	Non-Application Layer Protocol (T1095) 
•	Ingress Tool Transfer (T1105) 


53.	Rare URL Category Access Detected (Baseline Deviation)

Description –
This analytic rule detects access to rarely observed URL categories based on a historical baseline.
The rule builds a 7-day baseline of URL category frequency from Palo Alto THREAT logs and identifies categories that are infrequently accessed across the environment. It then monitors the last 1 hour of activity and triggers when users access URL categories whose occurrence falls below a defined threshold (e.g., < 500 events).
This behavior may indicate:
•	Access to newly emerging or uncommon web categories 
•	Malicious or suspicious websites not frequently seen in the environment 
•	Phishing, malware, or command-and-control (C2) activity using low-prevalence categories 
•	User risk behavior or policy evasion 
This detection is environment-aware and adapts dynamically based on observed traffic patterns, reducing false positives compared to static category-based alerts.
Query –
// --- Step 1: Build baseline (7 days) ---
let CategoryBaseline =
    CommonSecurityLog
    | where TimeGenerated >= ago(7d)
    | where Activity contains "THREAT"
    | where DeviceEventClassID contains "url"
    | where DeviceAction contains "alert"
    | extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
    | extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
    | extend URLCategoryArray = split(URLCategoryRaw, ",")
    | mv-expand URLCategoryArray
    | extend URLCategory = trim(@" """, tostring(URLCategoryArray))
    | summarize CategoryCount = count() by URLCategory;
// --- Step 2: Detection (1 hour) ---
CommonSecurityLog
| where TimeGenerated >= ago(1h)
| where Activity contains "THREAT"
| where DeviceEventClassID == "url"
| where DeviceAction contains "alert"
// --- Extract Category ---
| extend URLCategoryRaw = extract(@"PanOSURLCatList=""?([^;]*)", 1, AdditionalExtensions)
| extend URLCategoryRaw = trim(@" """, tolower(URLCategoryRaw))
| extend URLCategoryArray = split(URLCategoryRaw, ",")
| mv-expand URLCategoryArray
| extend URLCategory = trim(@" """, tostring(URLCategoryArray))
// --- Join with baseline ---
| join kind=inner CategoryBaseline on URLCategory
// --- Filter rare categories (dynamic threshold instead of top 10) ---
| where CategoryCount < 500   // 🔥 Tune this based on your data
// --- Output ---
| project
    TimeGenerated,
    DeviceName,
    SourceIP,
    DestinationIP,
    RequestURL,
    URLCategory,
    CategoryCount,
    DeviceAction,
    SourceUserName
 

MITRE ATT&CK Mapping
🎯 Primary Techniques
•	T1071
Adversaries may use web protocols and uncommon categories for C2 communication. 
•	T1566
Rare categories may include phishing or newly registered domains. 
________________________________________
🎯 Secondary / Supporting Techniques
•	T1204
User accessing suspicious or uncommon web content. 
•	T1105
Malware delivery via uncommon or low-frequency categories. 

54.	Potential Data Exfiltration via File Transfer Applications (High Outbound Volume)

Description –
This analytic rule detects potential data exfiltration activity by identifying internal hosts transferring significant volumes of data to external destinations using file-transfer-related application protocols.
The rule analyzes network traffic logs (CommonSecurityLog) to detect:
•	Use of file transfer applications (e.g., messaging platforms, file-sharing services) 
•	Internal-to-external communication 
•	High outbound data volume (in GB) 
•	Repeated connections within a 24-hour window 
By focusing on actual data movement (SentBytes) rather than just application usage, this rule highlights behaviors consistent with:
•	Unauthorized data transfers 
•	Insider data exfiltration 
•	Malware-driven data staging or extraction 
•	Use of unsanctioned file-sharing services 
This is a behavioral detection that does not rely on known malicious indicators, making it effective against unknown or emerging threats.

Query –
let TimeWindow = 24h;
let MinConnections = 3;
let MinDataGB = 0.05;   // ~50 MB equivalent
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where DeviceAction == "allow"
// --- File transfer apps ---
| where ApplicationProtocol contains "file"
// --- Internal to External ---
| where ipv4_is_private(SourceIP)
| where not(ipv4_is_private(DestinationIP))
// --- Traffic volume in GB ---
| extend SentBytes = tolong(SentBytes)
| extend SentGB = SentBytes / 1024.0 / 1024.0 / 1024.0
// --- Aggregate ---
| summarize
    ConnectionCount = count(),
    TotalSentGB = sum(SentGB),
    Destinations = dcount(DestinationIP),
    DestinationList = make_set(DestinationIP, 10),
    Applications = make_set(ApplicationProtocol, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP
// --- Detection thresholds ---
| where ConnectionCount >= MinConnections
| where TotalSentGB >= MinDataGB
| order by TotalSentGB desc
 

MITRE ATT&CK Mapping
🎯 Primary Techniques
•	T1041
Data transferred through application-layer protocols (file transfer apps)
•	T1567
Use of web-based file-sharing or messaging platforms for exfiltration
________________________________________
🎯 Secondary Techniques
•	T1071
Abuse of legitimate application protocols for data transfer 
•	T1020
Repeated transfers over time indicating automated behavior





55.	Suspicious Data Exfiltration via High-Risk File Sharing Applications

Description –
This analytic rule detects potential data exfiltration by identifying internal hosts transferring unusually large volumes of data to external destinations using high-risk file-sharing and transfer applications.
The rule analyzes Palo Alto TRAFFIC logs in Microsoft Sentinel and focuses on:
•	Internal → External communication 
•	High outbound data volume (SentBytes) 
•	Use of unsanctioned or high-risk applications such as FTP, WebDAV, Dropbox, Telegram, and similar services 
•	Behavioral indicators including multiple connections and multiple external destinations within a defined time window 
This detection helps identify:
•	Unauthorized data transfers 
•	Insider threats 
•	Malware-driven exfiltration 
•	Policy violations involving unapproved file-sharing tools
Query –
let TimeWindow = 24d;
let DataThresholdMB = 1000;        // Tune: 1000MB baseline
let MinConnections = 5;
// --- File Sharing / Exfil Apps ---
let FileSharingApps = dynamic([
    "dropbox-base",
    "boxnet-base",
    "ftp",
    "webdav",
    "tftp",
    "whatsapp-file-transfer",
    "telegram-base"
]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
// --- Core filters ---
| where Activity == "TRAFFIC"
| where DeviceAction == "allow"
// --- Internal → External ---
| where ipv4_is_private(SourceIP)
| where not(ipv4_is_private(DestinationIP))
// --- File sharing apps only ---
| where ApplicationProtocol in (FileSharingApps)
// --- Normalize traffic ---
| extend SentMB = coalesce(SentBytes, 0) / 1024 / 1024
| extend ReceivedMB = coalesce(ReceivedBytes, 0) / 1024 / 1024
| extend TrafficMB = SentMB + ReceivedMB
// --- Aggregate behavior ---
| summarize
    TotalOutboundMB = sum(SentMB),
    TotalTrafficMB = sum(TrafficMB),
    ConnectionCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 10),
    Applications = make_set(ApplicationProtocol, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, bin(TimeGenerated, 15m)
// --- Threshold logic ---
| where TotalOutboundMB >= DataThresholdMB
| where ConnectionCount >= MinConnections
// --- Optional noise reduction ---
| where UniqueDestinations >= 1
// --- Output ---
| project
    TimeGenerated,
    SourceIP,
    TotalOutboundMB,
    TotalTrafficMB,
    ConnectionCount,
    UniqueDestinations,
    Destinations,
    Applications,
    FirstSeen,
    LastSeen
| order by TotalOutboundMB desc
 

✅ MITRE ATT&CK Mapping
🔹 	Primary Technique
Exfiltration Over Web Service 
Data exfiltration using cloud storage or web-based services (Dropbox, Google Drive, etc.) 
🔹 Supporting Techniques
•	Exfiltration Over Alternative Protocol 
o	Use of FTP, WebDAV, or non-standard protocols for data transfer 
•	Exfiltration Over C2 Channel 
o	Data exfiltration via established communication channels (e.g., Telegram) 
🔹 Tactics
•	Exfiltration 

56.	Remote Access Tool Usage Between External Hosts (Public-to-Public Traffic)

Severity – Medium
Description –
This analytic rule detects the use of high-risk remote access tools (such as TeamViewer, AnyDesk, and Chrome Remote Desktop) in public-to-public network traffic observed by the firewall.
The rule analyzes Palo Alto TRAFFIC logs and identifies sessions where both the source and destination IP addresses are external (non-private), and the communication involves known remote access applications.
While this traffic does not directly involve internal assets, it may indicate:
•	External infrastructure leveraging remote access tools 
•	Potential relay or proxy behavior through monitored network paths 
•	Suspicious or unauthorized third-party remote access activity traversing the perimeter 
This rule is primarily useful for:
•	Visibility into remote access tool usage across external networks 
•	Detecting unusual traffic patterns passing through perimeter devices 
•	Supporting threat hunting and network monitoring scenarios🔹 Primary Technique
•	Remote Services
Use of remote access tools to connect to systems 
🔹 Supporting Techniques
•	Ingress Tool Transfer
Tools transferred or used across systems for remote control 
🔹 Tactics
•	Command and Control 
•	Lateral Movement 
Query –
let TimeWindow = 24h;
let MinConnections = 5;
let MinTargets = 2;
// --- High-Risk Remote Access Tools ---
let RemoteAccessApps = dynamic([
    "teamviewer-base",
    "anydesk",
    "chrome-remote-desktop",
    "ultraviewer",
    "remoteview",
    "splashtop-remote",
    "rustdesk-remote-desktop"
]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
// --- Core filters ---
| where Activity contains "TRAFFIC"
| where DeviceAction == "allow"
// --- External → Internal ---
| where not(ipv4_is_private(SourceIP))
| where not(ipv4_is_private(DestinationIP))
// --- Remote access tools ---
| where ApplicationProtocol in (RemoteAccessApps)
// | distinct ApplicationProtocol
// --- Aggregate behavior ---
| summarize
    ConnectionCount = count(),
    UniqueTargets = dcount(DestinationIP),
    Targets = make_set(DestinationIP, 10),
    Applications = make_set(ApplicationProtocol, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, bin(TimeGenerated, 10m)
 

57.	Unauthorized Remote Access Tool Usage from External to Internal Network

Severity – Medium
Description –
This analytic rule detects potential unauthorized remote access activity where external hosts initiate connections to internal systems using high-risk remote access tools such as TeamViewer, AnyDesk, and Chrome Remote Desktop.
The rule analyzes Palo Alto TRAFFIC logs to identify:
•	External (public) source IP addresses 
•	Internal (private) destination IP addresses 
•	Allowed traffic using known remote access applications 
Such behavior may indicate:
•	Unauthorized remote control attempts 
•	Compromised systems being accessed externally 
•	Third-party or attacker-controlled remote sessions 
•	Potential initial access or persistence mechanisms 
This detection is critical for identifying direct external access into internal environments using non-standard remote tools.
🔹 MITRE ATT&CK Mapping
🔹 Primary Technique
•	Remote Services
Remote access into internal systems 
🔹 Supporting Techniques
•	External Remote Services
External systems accessing internal resources 
•	Command and Control over Application Layer Protocol
Remote tools leveraging application protocols 
🔹 Tactics
•	Initial Access 
•	Command and Control 
•	Persistence

Query –
let TimeWindow = 24h;
let MinConnections = 5;
let MinTargets = 2;
// --- High-Risk Remote Access Tools ---
let RemoteAccessApps = dynamic([
    "teamviewer-base",
    "anydesk",
    "chrome-remote-desktop",
    "ultraviewer",
    "remoteview",
    "splashtop-remote",
    "rustdesk-remote-desktop"
]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
// --- Core filters ---
| where Activity contains "TRAFFIC"
| where DeviceAction == "allow"
// --- External → Internal ---
| where not(ipv4_is_private(SourceIP))
| where ipv4_is_private(DestinationIP)
// --- Remote access tools ---
| where ApplicationProtocol in (RemoteAccessApps)
// | distinct ApplicationProtocol
// --- Aggregate behavior ---
| summarize
    ConnectionCount = count(),
    UniqueTargets = dcount(DestinationIP),
    Targets = make_set(DestinationIP, 10),
    Applications = make_set(ApplicationProtocol, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, bin(TimeGenerated, 10m)

58.	Repeated High-Risk Threat Detection on Same Host (Palo Alto)

Description –
Detects repeated triggering of high-confidence Palo Alto threat signatures on the same internal host within a 1-hour window.
This rule focuses on attack-driven detections such as brute force attempts, exploitation activity, command-and-control communication, and credential abuse, while excluding low-value protocol anomalies and benign file-type events.
A high frequency of identical threat signatures from a single host may indicate:
•	Active compromise or malware persistence 
•	Ongoing brute force or credential abuse attempts 
•	Exploit retries against internal or external systems 
•	Command-and-control (C2) beaconing behavior 
This helps identify hosts exhibiting sustained malicious behavior that requires immediate investigation.



Query –
let timeframe = 1h;
let threshold = 10;
CommonSecurityLog
| where TimeGenerated >= ago(timeframe)
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
// ✅ Remove junk
| where DeviceEventClassID !in ("url", "file", "-9999")
| where not(
            DeviceEventClassID has_any (
    "File", "PDF", "PNG", "JPG", "GIF", "XML", "DLL", "EXE", "TXT", "CSV", "MP4", "MP3", "ZIP", "RAR"
    )
        )
// ✅ Keep only meaningful threats
| where DeviceEventClassID has_any (
    "Brute Force",
    "Remote Code Execution",
    "DCSync",
    "DNS Tunnel",
    "Exploit"
    )
// ✅ Normalize
| extend
    ThreatName_custom = DeviceEventClassID,
    MachineIdentifier_custom = SourceIP
| where isnotempty(ThreatName_custom)
| where isnotempty(MachineIdentifier_custom)
| where DeviceAction in ("alert", "blocked", "block-url", "reset-both")
// ✅ Dedup
| summarize arg_max(TimeGenerated, *) 
    by
    ThreatName_custom,
    MachineIdentifier_custom,
    DeviceAction,
    bin(TimeGenerated, 1m)
// ✅ Aggregate
| summarize
    EventCount = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    Actions = make_set(DeviceAction)
    by MachineIdentifier_custom, ThreatName_custom
| where EventCount >= threshold
| project MachineIdentifier_custom, ThreatName_custom, EventCount, FirstSeen, LastSeen
| sort by EventCount desc

🎯 MITRE ATT&CK Mapping
✅ Primary Techniques (High Confidence)
Tactic	Technique	ID
Credential Access	Brute Force	T1110
Credential Access	OS Credential Dumping (DCSync)	T1003.006
Command and Control	Application Layer Protocol	T1071
Command and Control	Ingress Tool Transfer / C2 Communication	T1105
Exfiltration	Exfiltration Over C2 Channel	T1041
Discovery	Account Discovery / Enumeration	T1087
Initial Access	Exploit Public-Facing Application	T1190

⚠️ Secondary / Contextual Techniques
Tactic	Technique	ID
Discovery	Network Service Discovery	T1046
Discovery	System Network Configuration Discovery	T1016
Lateral Movement	Remote Services (SMB / RPC abuse)	T1021
Collection	Data from Local System	T1005

59.	Palo Alto - possible internal to external port scanning

Description –
Identifies a list of internal Source IPs (10.x.x.x Hosts) that have triggered 10 or more non-graceful tcp server resets from one or more Destination IPs which results in an "ApplicationProtocol = incomplete" designation. The server resets coupled with an "Incomplete" ApplicationProtocol designation can be an indication of internal to external port scanning or probing attack. References: https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000ClUvCAK and https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000ClTaCAK

Query –
CommonSecurityLog
| where isnotempty(DestinationPort) and DeviceAction !in ("reset-both", "deny")
// filter out common usage ports. Add ports that are legitimate for your environment
| where DestinationPort !in ("443", "53", "389", "80", "0", "880", "8888", "8080")
| where ApplicationProtocol == "incomplete"
// filter out IANA ephemeral or negotiated ports as per https://en.wikipedia.org/wiki/Ephemeral_port
| where DestinationPort !between (toint(49512) .. toint(65535))
| where Computer != ""
| where ipv4_is_private(DestinationIP) == false
| extend Reason = coalesce(
                              column_ifexists("Reason", ""),
                              extract("reason=(.+?)(;|$)", 1, AdditionalExtensions),
                              ""
                          )
// Filter out any graceful reset reasons of AGED OUT which occurs when a TCP session closes with a FIN due to aging out.
| where Reason !has "aged-out"
// Filter out any TCP FIN which occurs when a TCP FIN is used to gracefully close half or both sides of a connection.
| where Reason !has "tcp-fin"
// Uncomment one of the following where clauses to trigger on specific TCP reset reasons
// See Palo Alto article for details - https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA10g000000ClUvCAK
// TCP RST-server - Occurs when the server sends a TCP reset to the client
// | where AdditionalExtensions has "reason=tcp-rst-from-server"
// TCP RST-client - Occurs when the client sends a TCP reset to the server
// | where AdditionalExtensions has "reason=tcp-rst-from-client"
// Already performed
//| extend reason = tostring(split(AdditionalExtensions, ";")[3])
| summarize StartTimeUtc = min(TimeGenerated), EndTimeUtc = max(TimeGenerated), count() by DeviceName, SourceUserID, SourceIP, ApplicationProtocol, Reason, DestinationPort, Protocol, DeviceVendor, DeviceProduct, DeviceAction, DestinationIP
| where count_ >= 10
| summarize StartTimeUtc = min(StartTimeUtc), EndTimeUtc = max(EndTimeUtc), makeset(DestinationIP), totalcount = sum(count_) by DeviceName, SourceUserID, SourceIP, ApplicationProtocol, Reason, DestinationPort, Protocol, DeviceVendor, DeviceProduct, DeviceAction
 

MITRE ATT&CK
Discovery - T1046 - Network Service Discovery

60.	Palo Alto - potential beaconing detected

Description –
Identifies beaconing patterns from Palo Alto Network traffic logs based on recurrent timedelta patterns. The query leverages various KQL functions to calculate time deltas and then compares it with total events observed in a day to find percentage of beaconing. This outbound beaconing pattern to untrusted public networks should be investigated for any malware callbacks or data exfiltration attempts. Reference Blog: http://www.austintaylor.io/detect/beaconing/intrusion/detection/system/command/control/flare/elastic/stack/2017/06/10/detect-beaconing-with-flare-elasticsearch-and-intrusion-detection-systems/ https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/detect-network-beaconing-via-intra-request-time-delta-patterns/ba-p/779586
Query –
let starttime = 2d;
let endtime = 1d;
let TimeDeltaThreshold = 25;
let TotalEventsThreshold = 30;
let MostFrequentTimeDeltaThreshold = 25;
let PercentBeaconThreshold = 80;
CommonSecurityLog
| where DeviceVendor == "Palo Alto Networks" and Activity == "TRAFFIC"
| where TimeGenerated between (startofday(ago(starttime))..startofday(ago(endtime)))
| where ipv4_is_private(DestinationIP)== false
| project TimeGenerated, DeviceName, SourceUserID, SourceIP, SourcePort, DestinationIP, DestinationPort, ReceivedBytes, SentBytes
| sort by SourceIP asc,TimeGenerated asc, DestinationIP asc, DestinationPort asc
| serialize
| extend nextTimeGenerated = next(TimeGenerated, 1), nextSourceIP = next(SourceIP, 1)
| extend TimeDeltainSeconds = datetime_diff('second',nextTimeGenerated,TimeGenerated)
| where SourceIP == nextSourceIP
//Whitelisting criteria/ threshold criteria
| where TimeDeltainSeconds > TimeDeltaThreshold
| summarize count(), sum(ReceivedBytes), sum(SentBytes)
by TimeDeltainSeconds, bin(TimeGenerated, 1h), DeviceName, SourceUserID, SourceIP, DestinationIP, DestinationPort
| summarize (MostFrequentTimeDeltaCount, MostFrequentTimeDeltainSeconds) = arg_max(count_, TimeDeltainSeconds), TotalEvents=sum(count_), TotalSentBytes = sum(sum_SentBytes), TotalReceivedBytes = sum(sum_ReceivedBytes)
by bin(TimeGenerated, 1h), DeviceName, SourceUserID, SourceIP, DestinationIP, DestinationPort
| where TotalEvents > TotalEventsThreshold and MostFrequentTimeDeltaCount > MostFrequentTimeDeltaThreshold
| extend BeaconPercent = MostFrequentTimeDeltaCount/toreal(TotalEvents) * 100
| where BeaconPercent > PercentBeaconThreshold

MITRE ATT&CK
•	Command And Control
o	T1071 - Application Layer Protocol
o	T1571 - Non-Standard Port

61.	Microsoft COVID-19 file hash indicator matches

Description –
Identifies a match in CommonSecurityLog Event data from any FileHash published in the Microsoft COVID-19 Threat Intel Feed - as described at https://www.microsoft.com/security/blog/2020/05/14/open-sourcing-covid-threat-intelligence/

Query –
let dt_lookBack = 1h;
let covidIndicators = (externaldata(TimeGenerated:datetime, FileHashValue:string, FileHashType: string, TlpLevel: string, Product: string, ThreatType: string, Description: string )
[@"https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/Sample%20Data/Feeds/Microsoft.Covid19.Indicators.csv"] with (format="csv"));
let fileHashIndicators = covidIndicators
| where isnotempty(FileHashValue);
// Handle matches against both lower case and uppercase versions of the hash:
(fileHashIndicators | extend FileHashValue = tolower(FileHashValue)
| union (fileHashIndicators | extend FileHashValue = toupper(FileHashValue)))
// using innerunique to keep perf fast and result set low, we only need one match to indicate potential malicious activity that needs to be investigated
|  join kind=innerunique (
   CommonSecurityLog | where TimeGenerated >= ago(dt_lookBack)
   | where isnotempty(FileHash)
   | extend CommonSecurityLog_TimeGenerated = TimeGenerated
   )
on $left.FileHashValue == $right.FileHash
| summarize CommonSecurityLog_TimeGenerated = arg_max(CommonSecurityLog_TimeGenerated, *) by FileHashValue
| project CommonSecurityLog_TimeGenerated, FileHashValue, FileHashType, Description, ThreatType,
SourceIP, SourcePort, DestinationIP, DestinationPort, SourceUserID, SourceUserName, DeviceName, DeviceAction,
RequestURL, DestinationUserName, DestinationUserID, ApplicationProtocol, Activity
| extend AccountName = tostring(split(SourceUserName, "@")[0]), AccountUPNSuffix = tostring(split(SourceUserName, "@")[1])
| extend HostName = tostring(split(DeviceName, ".")[0]), DomainIndex = toint(indexof(DeviceName, '.'))
| extend HostNameDomain = iff(DomainIndex != -1, substring(DeviceName, DomainIndex + 1), DeviceName)

MITRE ATT&CK
•	Execution
o	T1204 - User Execution
o	T1204.002 - Malicious File


62.	 Unauthorised remote access tool usage from Internal to Internal Network

Description-
Detects internal-to-internal use of remote access tools (e.g., TeamViewer, AnyDesk, UltraViewer) based on allowed network traffic. The rule identifies repeated connections from a source host to one or more internal systems, which may indicate unauthorized remote administration or lateral movement.
Results should be validated against known administrative or helpdesk activity. Unusual patterns—such as access to multiple hosts or unexpected source systems—may indicate malicious behavior.

Query:
let TimeWindow = 3d;
let MinConnections = 3;
let MinTargets = 1;
let RemoteAccessApps = dynamic([
    "teamviewer-base",
    "anydesk",
    "chrome-remote-desktop",
    "ultraviewer",
    "remoteview",
    "splashtop-remote",
    "rustdesk-remote-desktop"
]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where DeviceAction == "allow"
// Internal → Internal
| where ipv4_is_private(SourceIP)
| where ipv4_is_private(DestinationIP)
// RAT tools
| where ApplicationProtocol in (RemoteAccessApps)
// FIXED extend
| extend TrafficVolume = coalesce(SentBytes,0) + coalesce(ReceivedBytes,0)
| extend TrafficMB = TrafficVolume / 1024.0 / 1024.0
| summarize
    ConnectionCount = count(),
    UniqueTargets = dcount(DestinationIP),
    Targets = make_set(DestinationIP, 10),
    Applications = make_set(ApplicationProtocol, 10),
    TotalTrafficMB = sum(TrafficMB),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DeviceName, bin(TimeGenerated, 10min)
| where ConnectionCount >= MinConnections
| where UniqueTargets >= MinTargets
| sort by ConnectionCount desc
 

MITRE ATT&CK Mapping
•	Tactic: Lateral Movement (TA0008) 
•	Technique: Remote Services (T1021) 
Additional Context:
•	Remote Access Software (T1219) — use of tools like TeamViewer or AnyDesk for remote control


63.	Suspicious Crypto Mining Network Traffic via Known Malicious Ips

Description
Detects network traffic between internal hosts and known or high-confidence malicious IP addresses associated with cryptocurrency mining activity. The rule leverages threat intelligence (confidence ≥75) combined with mining-related indicators (e.g., mining pools, stratum ports, or keywords) to identify potential mining communication.
This may indicate compromised systems performing unauthorized cryptocurrency mining or communicating with mining infrastructure.
Query
let TimeWindow = 15m;
// --- Threat Intelligence (X-Force equivalent) ---
let MaliciousIPs =
ThreatIntelligenceIndicator
| where TimeGenerated >= ago(7d)
| where ConfidenceScore >= 75
| where ThreatType has_any ("Botnet","Crypto","Malware","C2","Mining")
| summarize by NetworkIP;
// --- Network Traffic ---
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where DeviceAction == "allow"
// Exclude internal-to-internal
| where not(ipv4_is_private(SourceIP) and ipv4_is_private(DestinationIP))
// Match TI (source OR destination)
| where SourceIP in (MaliciousIPs) or DestinationIP in (MaliciousIPs)
// Crypto indicators
| where RequestURL has_any ("pool","xmr","monero","mining","stratum")
    or DestinationPort in (3333,4444,5555,7777)
// Aggregate
| summarize
    ConnectionCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 10),
    URLs = make_set(RequestURL, 10),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DeviceName, bin(TimeGenerated, 5m)
| where ConnectionCount > 5
| sort by ConnectionCount desc
 

MITRE ATT&CK Mapping
•	Tactic: Command and Control (TA0011) 
•	Technique: 
o	T1071 – Application Layer Protocol (use of common protocols for mining communication) 
Additional Context:
•	T1496 – Resource Hijacking (cryptocurrency mining activity)


64.	Exploit Attempt Followed by Cryptocurrency Mining Activity

Description
Detects a sequence where a host is targeted by an exploit attempt (based on threat intelligence and exploit-related signatures) and subsequently executes cryptocurrency mining activity within a defined time window.
This correlation indicates a likely compromise where an attacker exploits a vulnerability and deploys a crypto miner as a post-exploitation payload.

Query
let TimeWindow = 30m;
// --- Threat Intelligence ---
let MaliciousIPs =
ThreatIntelligenceIndicator
| where TimeGenerated >= ago(7d)
| where ConfidenceScore >= 75
| summarize by NetworkIP;
// --- Exploit Events ---
let ExploitEvents =
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "THREAT"
| where DeviceAction in ("alert","deny","drop","reset")
// Match malicious IPs
| where SourceIP in (MaliciousIPs) or DestinationIP in (MaliciousIPs)
// Broad exploit indicators
| where DeviceEventClassID has_any ("CVE","exploit","injection","overflow","command")
| project
    ExploitTime = TimeGenerated,
    DestinationIP,
    SourceIP,
    DeviceName,
    Signature = DeviceEventClassID;
// --- Crypto Mining Process (reuse your rule logic) ---
let CryptoMiningEvents =
SecurityEvent
| where TimeGenerated >= ago(TimeWindow)
| where EventID in (4688,1)
| where isnotempty(CommandLine)
| where CommandLine matches regex @"(?i)(miner|coin|monero|bitcoin|cryptonight)"
| project
    MiningTime = TimeGenerated,
    Computer,
    Account = SubjectUserName,
    CommandLine;
// --- Correlation ---
ExploitEvents
| join kind=inner (
    CryptoMiningEvents
) on $left.DestinationIP == $right.Computer
// Sequence logic
| where MiningTime > ExploitTime
| where MiningTime <= ExploitTime + TimeWindow
| project
    ExploitTime,
    MiningTime,
    SourceIP,
    DestinationIP,
    Computer,
    Account,
    Signature,
    CommandLine
| sort by MiningTime desc
 

MITRE ATT&CK Mapping
•	Tactic: 
o	Initial Access (TA0001) 
o	Execution (TA0002) 
o	Impact (TA0040) 
•	Techniques: 
o	T1190 – Exploit Public-Facing Application 
o	T1059 – Command and Scripting Interpreter (execution of mining payload) 
o	T1496 – Resource Hijacking (cryptocurrency mining







65.	High Volume Denied Traffic to Single Destination (Potential Targeted Attack / Misconfiguration)

Description – 
This analytic rule detects scenarios where a non-server internal host generates an unusually high volume of denied or dropped traffic toward a single destination IP within a short time window (5 minutes).
The rule analyzes Palo Alto TRAFFIC logs and focuses on events where the firewall action is deny, drop, or drop-packet, indicating blocked communication attempts.
It identifies behavior where:
•	A single source IP sends a large number of connection attempts (over 400) 
•	All attempts are directed toward only one destination IP 
•	The activity occurs within a 5-minute window 
•	Known infrastructure systems (e.g., MECM, Domain Controllers, DHCP servers, critical assets) are excluded via watchlists 
Query –
let TimeWindow = 5m;
let Threshold = 400;
// --- Server Watchlist ---
let ServerIPs = union
        (_GetWatchlist('Mecm_Servers')),
        (_GetWatchlist('Domain Controllers')),
        (_GetWatchlist('DHCP_SERVERS_IP')),
        (_GetWatchlist('Critical_assets'))
    | project IP = tostring(SearchKey);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where DeviceAction in ("deny", "drop", "drop-packet")
| where SourceIP !in (ServerIPs)
// --- Normalize ---
| extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
// --- Aggregate with Context ---
| summarize 
    EventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 3),
    Ports = make_set(DestinationPort, 5),
    Applications = make_set(ApplicationProtocol, 5),
    Rules = make_set(PanOSRuleUUID_CF, 5),
    TotalTrafficMB = sum(TrafficVolume) / 1024 / 1024,
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, bin(TimeGenerated, TimeWindow)
// --- QRadar Logic ---
| where EventCount > Threshold
| where UniqueDestinations == 1
 



🧬 MITRE ATT&CK Mapping
Tactic	Technique	ID
Discovery	Network Service Scanning	T1046
Credential Access	Brute Force	T1110
Command and Control	Application Layer Protocol	T1071
Lateral Movement	Remote Services	T1021

66.	Potential Internal Worm Propagation via SMB/File Activity

Description – 
This analytic rule detects worm-like propagation behavior originating from an internal host by identifying high-volume file or SMB-based activity targeting a large number of unique destination systems within a short time window.
The rule analyzes Palo Alto THREAT logs (file-based events) and focuses on internal sources initiating connections over SMB, FTP, or file-transfer protocols, which are commonly abused for lateral movement and worm spread.
It identifies behavior where:
•	A single internal source IP generates multiple file-related threat events (>5) 
•	The activity spreads across more than 300 unique destination IPs 
•	All activity occurs within a 20-minute window 
•	Known benign systems (servers, scanners, MECM, etc.) are excluded 
•	Traffic involves SMB ports (445/139) or file-transfer protocols 
Query –
let TimeWindow = 20m;
let MinEvents = 5;
let MinDestinations = 300;
// --- Optional: Simulated Watchlists (replace with real watchlists) ---
let ExcludedIPs = dynamic([
    "10.10.10.10",   // Servers
    "10.20.20.20",   // Scanners
    "10.30.30.30"    // MECM / VA / LS scanners
]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
// --- Scope to Palo Alto Threat/File Logs ---
| where DeviceVendor == "Palo Alto Networks"
| where Activity == "THREAT"
| where DeviceEventClassID == "file"
// --- Focus on internal source (worm origin) ---
| where ipv4_is_private(SourceIP)
// --- Exclude known benign/scanner sources ---
| where SourceIP !in (ExcludedIPs)
// --- High-risk propagation indicators ---
| where ApplicationProtocol has_any ("smb", "ftp", "ms-ds-smbv3")
    or DestinationPort in (445, 139)
// --- Normalize ---
| extend TimeBin = bin(TimeGenerated, TimeWindow)
| extend FileIndicator = tostring(FileName)
// --- Aggregate behavior ---
| summarize
    EventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 1000),
    FileNames = make_set(FileIndicator, 50),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, TimeBin
// --- Threshold enforcement ---
| where EventCount > MinEvents
| where UniqueDestinations > MinDestinations
// --- Output shaping ---
| project
    TimeBin,
    SourceIP,
    EventCount,
    UniqueDestinations,
    Destinations,
    FileNames,
    FirstSeen,
    LastSeen
| order by UniqueDestinations desc
 

🧬 MITRE ATT&CK Mapping
Tactic	Technique	ID
Lateral Movement	Remote Services (SMB)	T1021.002
Command and Control	Ingress Tool Transfer	T1105
Execution	Exploitation of Remote Services	T1210
Discovery	Network Service Scanning	T1046

67.	Unblocked Internal Worm Activity

Description –
This analytic detects worm-related threat events observed in internal-to-internal network traffic where the firewall did not enforce a blocking action.
The rule identifies malware signatures containing "worm" in threat logs and focuses on private-to-private IP communication, indicating possible lateral movement within the network.
By excluding events where the firewall actively reset the connection (e.g., reset-both), this detection highlights potentially successful propagation attempts, policy gaps, or delayed detection scenarios where malicious activity may have completed before enforcement.
Additional enrichment fields such as threat category, antivirus content version, and firewall rule UUID are extracted to support investigation and response.
CommonSecurityLog 
| where Activity == "THREAT"
| where tolower(DeviceEventClassID) contains "worm" 
// Internal lateral movement only 
| where ipv4_is_private(SourceIP) 
| where ipv4_is_private(DestinationIP) 
// ONLY detections that were NOT blocked 
| where isempty(DeviceAction) 
// Extract useful PAN fields 
| extend
    ThreatCategory = extract(@"PanOSThreatCategory=([^;]+)", 1, AdditionalExtensions),
    ContentVersion = extract(@"PanOSContentVer=([^;]+)", 1, AdditionalExtensions),
    RuleUUID = extract(@"PanOSRuleUUID=([^;]+)", 1, AdditionalExtensions),
    HighResTime = extract(@"PanTimeHighRes=([^;]+)", 1, AdditionalExtensions) 
// Output 
| project
    TimeGenerated,
    SourceIP,
    DestinationIP,
    DeviceName,
    DeviceEventClassID,
    ContentVersion,
    RuleUUID,
    DeviceAction

🧬 MITRE ATT&CK Mapping
🔹 Primary Techniques
🟠 T1021 – Remote Services
•	Worms commonly propagate via: 
o	SMB 
o	FTP (seen in your logs) 
•	Internal host-to-host communication strongly aligns 
🟠 T1105 – Ingress Tool Transfer
•	Malware being transferred between systems 
•	Supported by: 
o	FTP protocol observed in logs 
o	Executable payloads (pe category) 
🟠 T1046 – Network Service Discovery
•	Worm behavior often includes probing internal systems 
•	Even if not explicit scanning, propagation implies discovery 
🔹 Secondary Techniques
🟡 T1204 – User Execution
•	Worm payloads may require execution on target systems 
•	Relevant due to executable (.exe) involvement 
🟡 T1570 – Lateral Tool Transfer
•	Direct lateral movement via file transfer 
•	Strongly applicable in your dataset 

68.	Repeated Detection of Similar Threats Across Multiple Hosts

Description –
This analytic identifies situations where the same threat signature is observed on multiple hosts within the same network segment over a defined period of time.
The rule focuses on security-relevant detections (e.g., malware, exploit, or obfuscated script indicators) and excludes generic telemetry such as file inspection, URL filtering, and sandbox-related events.
While a single detection may represent normal background activity or isolated behavior, repeated occurrences of the same threat across multiple systems in the same subnet may indicate shared exposure, common configuration, or potential propagation activity, and may warrant further review.
Query –
let TimeWindow = 24h;
let MinHosts = 10;
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "THREAT"
// REMOVE NOISE
| where DeviceEventClassID !in ("file", "url", "wildfire")
// KEEP REAL THREATS
| where DeviceEventClassID has_any (
    "worm",
    "trojan",
    "virus",
    "malware",
    "exploit",
    "obfuscation",
    "metasploit"
)
// Normalize
| extend ThreatName = tostring(DeviceEventClassID)
| extend Host = SourceIP
| extend Subnet = strcat(split(SourceIP, ".")[0], ".", split(SourceIP, ".")[1], ".", split(SourceIP, ".")[2])
// Correlation
| summarize
    EventCount = count(),
    UniqueHosts = dcount(Host),
    Hosts = make_set(Host, 50),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by Subnet, ThreatName, bin(TimeGenerated, TimeWindow)
// Threshold
| where UniqueHosts >= MinHosts
// Output
| project
    Subnet,
    ThreatName,
    UniqueHosts,
    Hosts,
    EventCount,
    FirstSeen,
    LastSeen
 

🧬 MITRE ATT&CK Mapping
Primary
•	T1021 – Remote Services (potential lateral movement patterns) 
Secondary (contextual)
•	T1570 – Lateral Tool Transfer 
•	T1046 – Network Service Discovery 
👉 Note: Mapping is indicative and should be interpreted based on investigation context.

69.	Multiple User Account Lockouts Observed Across Environment

Description –
This analytic identifies instances where a high number of distinct user accounts are locked out within a defined time window.
The rule monitors Windows account lockout events (Event ID 4740) and aggregates activity across the environment to highlight situations where multiple users are affected in a short period. Additional context is included to help analysts understand:
•	The number of impacted users 
•	The systems involved in generating lockouts 
•	The domain controllers processing the events 
•	Whether the activity originates from a single or multiple source systems 
Such patterns may be associated with credential issues, automated processes, or authentication-related activity, and should be reviewed to determine whether the behavior is expected or requires further investigation.
Query –
let TimeWindow = 24h;
let MinUsers = 30;
SecurityEvent
| where TimeGenerated >= ago(TimeWindow)
| where EventID == 4740
| where isnotempty(TargetUserName)
| where TargetUserName !in~ ("ANONYMOUS LOGON", "SYSTEM", "LOCAL SERVICE", "NETWORK SERVICE")
| extend CallerHost = extract(@"Caller Computer Name:\s*([^\r\n]+)", 1, EventData)
| extend CallerHost = iff(isempty(CallerHost), "Unknown", CallerHost)
| extend
    Account = tostring(TargetUserName),
    Domain = tostring(TargetDomainName),
    LockedOnDC = tostring(Computer)
| summarize
    EventCount = count(),
    UniqueUsers = dcount(Account),
    UsersSample = make_set(Account, 50),
    Domains = make_set(Domain, 10),
    SourceHosts = make_set(CallerHost, 20),
    DistinctSources = dcount(CallerHost),
    DCsInvolved = make_set(LockedOnDC, 10),
    DistinctDCs = dcount(LockedOnDC),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by bin(TimeGenerated, TimeWindow)
| where UniqueUsers >= MinUsers
| extend
    BehaviorType = case(DistinctSources == 1, "Single source system (likely misconfiguration or cached credentials)", DistinctSources > 5, "Multiple source systems (requires investigation)", "Limited spread across few systems"),
    InfrastructureScope = case(DistinctDCs > 3, "Multiple domain controllers involved", DistinctDCs == 1, "Single domain controller", "Few domain controllers"),
    AlertSummary = strcat(UniqueUsers, " users locked out across ", DistinctSources, " source system(s) within ", tostring(TimeWindow))
| project
    TimeGenerated,
    UniqueUsers,
    EventCount,
    DistinctSources,
    DistinctDCs,
    UsersSample,
    SourceHosts,
    DCsInvolved,
    Domains,
    BehaviorType,
    InfrastructureScope,
    AlertSummary,
    FirstSeen,
    LastSeen

🧬 MITRE ATT&CK Mapping
🔹 Primary
•	T1110 – Brute Force
Potential alignment if multiple accounts are being targeted through repeated authentication attempts. 
________________________________________
🔹 Secondary (Contextual)
•	T1078 – Valid Accounts
May apply if legitimate credentials are being used or tested across accounts. 
70.	Excessive Denied SMB Traffic Indicating Potential Lateral Movement Attempt

Severity – Medium
Description
Detects a source host generating a high volume of denied SMB connections to multiple internal systems within a short time window, indicating potential failed lateral movement, worm propagation, or SMB brute-force activity.
Query
CommonSecurityLog
| where TimeGenerated >= ago(24h)
| where Activity == "TRAFFIC"
| where ApplicationProtocol contains "smb"
| where DeviceAction in ("deny", "denied", "drop", "blocked")
| extend TrafficVolume = coalesce(SentBytes, 0) + coalesce(ReceivedBytes, 0)
| summarize
    DeniedEventCount = count(),
    HostsTargeted = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 50),
    Applications = make_set(ApplicationProtocol, 10),
    TotalTraffic = sum(TrafficVolume),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, SourceUserName, DeviceName
| where DeniedEventCount > 100   // Threshold 1: volume
    or HostsTargeted > 10        // Threshold 2: spread
| extend
    Applications_str = strcat_array(Applications, ", "),
    Destinations_str = strcat_array(Destinations, ", "),
    TrafficMB = TotalTraffic / 1024 / 1024
| project
    SourceIP,
    SourceUserName,
    DeviceName,
    DeniedEventCount,
    HostsTargeted,
    TrafficMB,
    Applications_str,
    Destinations_str,
    FirstSeen,
    LastSeen
| order by DeniedEventCount desc

MITRE ATT&CK Mapping
Primary Technique
•	TA0008 – Lateral Movement 
o	T1021.002 – SMB/Windows Admin Shares 
Secondary Techniques
•	TA0007 – Discovery 
o	T1046 – Network Service Discovery 
•	TA0006 – Credential Access (possible context) 
o	T1110 – Brute Force
Watchlist required of SDwans to reduce false positives.

71.	Palo Alto - Outbound Spyware/Virus Activity Detected
New rule name – Palo Alto - Repeated Internal-to-External Spyware/Virus Threat Alerts
New description - 
This analytic rule detects repeated spyware or virus-related threat alerts generated by Palo Alto firewalls for outbound communications from an internal host to an external destination. A high volume of malware-related threat alerts within a short period may indicate an infected system attempting to communicate with malicious infrastructure, download additional payloads, or establish command-and-control (C2) communications.
The rule is designed to reduce noise from isolated threat detections and instead focus on sustained malicious activity patterns that may warrant immediate investigation.
Query Details:
• Monitors Palo Alto THREAT events classified as spyware or virus activity.
• Identifies outbound network connections originating from an internal host and targeting an external destination.
• Focuses on threat events where the firewall generated an alert, indicating suspicious or malicious activity was detected during the session.
• Correlates repeated threat detections between the same internal and external IP addresses within a five-minute period.
SOC Analyst Guidance:
Initial Validation
• Identify the affected internal host and external destination involved in the alert.
• Review the threat signatures and categories associated with the detected activity.
• Determine whether the destination is known, trusted, or associated with malicious infrastructure.
Investigation Steps
• Review historical activity from the source host to identify recurring communication patterns.
• Investigate endpoint security telemetry for evidence of malware execution, persistence, or s-suspicious processes.
• Validate whether additional internal hosts have communicated with the same destination.
• Review DNS, proxy, and network traffic logs for related indicators of compromise.
Common False Positives
• Security testing activities.
• Authorized malware simulation exercises.
• Internal security research or sandbox environments.
• Threat validation performed by security tools.
Escalation Guidance
Escalate when:
• Multiple threat signatures are observed from the same host.
• The destination IP is identified as malicious through threat intelligence sources.
• Endpoint evidence confirms malware execution or command-and-control activity.
• Similar activity is observed across multiple hosts.
Response Recommendations
• Isolate affected systems where compromise is suspected.
• Block confirmed malicious destinations.
• Perform endpoint malware analysis and remediation.
• Initiate incident response procedures if compromise is confirmed.

Severity – High
Query:
CommonSecurityLog
| where Activity contains "THREAT"
// --- Strict malware ---
| where DeviceEventCategory has_any ("spyware", "virus")
// --- Allowed traffic ---
| where DeviceAction == "alert"
// --- Outbound: internal → external ---
| where ipv4_is_private(SourceIP) and not(ipv4_is_private(DestinationIP))
// --- Parse PAN fields ---
| extend PanThreatCategory = extract(@"PanOSThreatCategory=([^;]+)", 1, AdditionalExtensions)
// --- Normalize ---
| extend ThreatName = tostring(DeviceEventClassID)
// --- Aggregate ---
| summarize 
    EventCount = count(),
    ThreatNames = make_set(ThreatName, 20),
    ThreatTypes = make_set(PanThreatCategory, 20)
    by SourceIP, DestinationIP, bin(TimeGenerated, 5m), DeviceEventCategory
| where EventCount >= 5
 

MITRE ATT&CK Mapping
Primary
•	TA0011 – Command and Control 
o	T1071 – Application Layer Protocol 
Secondary
•	TA0010 – Exfiltration 
o	T1041 – Exfiltration Over C2 Channel

72.	Inbound Reconnaissance Scan from External Source

Severity – High
Description 
Detects external IP addresses performing reconnaissance activity against internal network assets by identifying high-volume connection attempts across multiple internal hosts and ports within a defined time window. This behavior may indicate pre-attack scanning or service enumeration attempts.


Query
let TimeWindow = 24h;
// --- Known Internal/Authorized Scanners ---
let ScannerIPs = (
    _GetWatchlist('Tenable_scanners')
    | project SearchKey
);
// --- Detection Logic ---
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
// --- External → Internal Traffic ---
| where ipv4_is_private(SourceIP) == false
| where ipv4_is_private(DestinationIP) == true
// --- Exclude Known Scanners ---
| where SourceIP !in (ScannerIPs)
// --- Focus on Relevant Actions ---
| where DeviceAction in ("deny","blocked","alert","allow")
// --- Aggregate Behavior ---
| summarize
    EventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    UniquePorts = dcount(DestinationPort),
    DestinationIPs = strcat_array(make_set(DestinationIP, 200), ", "),
    DestinationPorts = strcat_array(make_set(DestinationPort, 50), ", "),
    DeviceActions = strcat_array(make_set(DeviceAction, 10), ", "),
    Protocols = strcat_array(make_set(Protocol, 10), ", "),
    DeviceProducts = strcat_array(make_set(DeviceProduct, 5), ", "),
    DeviceVendors = strcat_array(make_set(DeviceVendor, 5), ", "),
    Devices = strcat_array(make_set(DeviceName, 5), ", "),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated)
by SourceIP
// --- Thresholds (tunable) ---
| where EventCount > 20
| where UniqueDestinations > 30
| where UniquePorts > 10
// --- Final Output ---
| project
    StartTime,
    EndTime,
    SourceIP,
    EventCount,
    UniqueDestinations,
    UniquePorts,
    DestinationPorts,
    Protocols,
    DeviceActions,
    DeviceVendors,
    DeviceProducts,
    Devices,
    DestinationIPs
 

MITRE ATT&CK Mapping
Primary Technique
•	TA0043 – Reconnaissance 
Techniques
•	T1595 – Active Scanning 
o	T1595.001 – Scanning IP Blocks 
o	T1595.002 – Vulnerability Scanning

73.	Excessive Denied Connections to Sinkhole Domains

Description
Detects hosts generating repeated denied or blocked outbound connections to known sinkhole domains. Sinkhole infrastructure is commonly used to intercept malicious command-and-control (C2) traffic, indicating that an internal host may be infected and attempting to communicate with attacker-controlled infrastructure.
This rule identifies potential compromised systems by correlating firewall deny events with a curated watchlist of sinkhole domains and triggering when a threshold of repeated connection attempts is exceeded within a defined time window.
Query:
let SinkholeDomains = toscalar(
    _GetWatchlist('Sinkhole_Domains')
    | summarize make_set(tolower(tostring(SearchKey)))
);
CommonSecurityLog
| where TimeGenerated >= ago(1d)
| where Activity in ("TRAFFIC","THREAT")
| where DeviceAction in ("deny", "drop", "blocked")
| where isnotempty(RequestURL)
| extend Domain = tolower(tostring(parse_url(RequestURL).Host))
| where isnotempty(Domain)
| where Domain has_any (SinkholeDomains)
| summarize DenyCount = count(), Destinations = make_set(DestinationIP, 20) by SourceIP, bin(TimeGenerated, 5h)
| where DenyCount > 3
 

MITRE ATT&CK Mapping
Primary Techniques
•	T1071
Malicious traffic attempting outbound communication over application protocols 
•	T1568
Malware resolving domains (including sinkholed ones) 
Secondary Techniques
•	T1070
Repeated failed communication attempts may indicate evasion or fallback behavior 
•	T1046
In some cases, repeated denied connections may indicate scanning or discovery behavior



74.	WormDetection: Successful Connections to the Internet on Common Worm Ports
Description
Detects internal hosts making a high volume of successful outbound connections to external IP addresses over ports commonly associated with worm propagation (e.g., SMB, NetBIOS, RPC). This behavior is indicative of infected systems attempting to scan, propagate, or communicate externally.
New Description- 
This analytic rule detects repeated spyware or virus-related threat alerts generated by Palo Alto firewalls for outbound communications from an internal host to an external destination. A high volume of malware-related threat alerts within a short period may indicate an infected system attempting to communicate with malicious infrastructure, download additional payloads, or establish command-and-control (C2) communications.
The rule is designed to reduce noise from isolated threat detections and instead focus on sustained malicious activity patterns that may warrant immediate investigation.
Query detail:
1.	Monitors Palo Alto THREAT events classified as spyware or virus activity.
2.	Identifies outbound network connections originating from an internal host and targeting an external destination.
3.	Focuses on threat events where the firewall generated an alert, indicating suspicious or malicious activity was detected during the session.
4.	Correlates repeated threat detections between the same internal and external IP addresses within a five-minute period.
Additional considerations:
•	Initial Validation:
o	Identify the affected internal host and external destination involved in the alert.
o	Review the threat signatures and categories associated with the detected activity.
o	Determine whether the destination is known, trusted, or associated with malicious infrastructure.
•	Investigation Steps:
o	Review historical activity from the source host to identify recurring communication patterns.
o	Investigate endpoint security telemetry for evidence of malware execution, persistence, or suspicious processes.
o	Validate whether additional internal hosts have communicated with the same destination.
o	Review DNS, proxy, and network traffic logs for related indicators of compromise.
•	Common False Positives:
o	Security testing activities.
o	Authorized malware simulation exercises.
o	Internal security research or sandbox environments.
o	Threat validation performed by security tools.
•	Escalation Guidance:
o	Escalate when multiple threat signatures are observed from the same host.
o	Escalate when the destination IP is identified as malicious through threat intelligence sources.
o	Escalate when endpoint evidence confirms malware execution or command-and-control activity.
o	Escalate when similar activity is observed across multiple hosts.
•	Response Recommendations:
o	Isolate affected systems where compromise is suspected.
o	Block confirmed malicious destinations.
o	Perform endpoint malware analysis and remediation.
o	Initiate incident response procedures if compromise is confirmed.
________________________________________
Query :
let TimeWindow = 12h;
let MinConnections = 50;
let MinDestinations = 20;
let WormPorts = dynamic([135,137,138,139,445,1433,3389,4444,5555,1900]);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where Activity == "TRAFFIC"
| where ipv4_is_private(SourceIP)
| where not(ipv4_is_private(DestinationIP))
| where DeviceAction in ("allow", "allowed", "permit")
| where DestinationPort in (WormPorts)
| extend TimeBin = bin(TimeGenerated, TimeWindow)
| summarize
    ConnectionCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    Destinations = make_set(DestinationIP, 100),
    Ports = make_set(DestinationPort, 20),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by SourceIP, TimeBin
| where ConnectionCount > MinConnections
| where UniqueDestinations > MinDestinations
| project
    TimeBin,
    SourceIP,
    ConnectionCount,
    UniqueDestinations,
    Ports,
    Destinations,
    FirstSeen,
    LastSeen
| order by ConnectionCount desc

MITRE ATT&CK Mapping (Refined)
Primary Tactic: Lateral Movement
•	T1021
The detected behavior involves attempts to connect to remote systems over services such as SMB (445), NetBIOS (137–139), and RDP (3389), which are commonly leveraged by worms for propagation. 
Primary Tactic: Discovery
•	T1046
High-volume connections to multiple external hosts across specific ports indicate scanning behavior to identify reachable services for further exploitation. 
Secondary Tactic: Command and Control
•	T1071
Some worms use standard application-layer protocols to communicate externally after initial compromise. 

Rule Configuration
Rule Type - Scheduled
Query Frequency - Every 1 hour
Lookup Period - Last 12 hours
Trigger - Trigger when results > 0

75.	Successful Inbound Connection from Known Botnet C2 Infrastructure

Description:
Detects successful inbound connections from external IP addresses associated with known botnet or command-and-control infrastructure toward internal assets.
Query:
let BotnetIOC =
ThreatIntelligenceIndicator
| where Active == true
| where ExpirationDateTime > now()
| where Description has_any ("botnet","c2c","command and control","malware")
    or ThreatType has_any ("botnet","malware","C2")
| extend TI_IP = tostring(NetworkIP)
| where isnotempty(TI_IP)
| summarize LatestIndicator=max(TimeGenerated) by TI_IP;
CommonSecurityLog
| where TimeGenerated >= ago(15m)
| where Activity == "TRAFFIC"
| where DeviceVendor == "Palo Alto Networks"
| where DeviceAction in ("allow","alert")
| where not(ipv4_is_private(SourceIP))
| where ipv4_is_private(DestinationIP)
| lookup kind=inner BotnetIOC on $left.SourceIP == $right.TI_IP
| extend TrafficVolume = coalesce(SentBytes,0) + coalesce(ReceivedBytes,0)
| summarize
    EventCount=count(),
    FirstSeen=min(TimeGenerated),
    LastSeen=max(TimeGenerated),
    DestinationIPs=make_set(DestinationIP,20),
    DestinationPorts=make_set(DestinationPort,20),
    DeviceNames=make_set(DeviceName,10),
    Actions=make_set(DeviceAction,10),
    TotalTrafficBytes=sum(TrafficVolume)
by SourceIP
| where EventCount >= 0
| project
    FirstSeen,
    LastSeen,
    SourceIP,
    EventCount,
    DestinationIPs,
    DestinationPorts,
    Actions,
    DeviceNames,
    TotalTrafficBytes
 
Tactic	Technique
Command and Control	T1071
Initial Access	T1190
Malware	T1583
Botnet	T1584


76.	PA Series Critical Threat Traffic Detected
Description:
Detects high-confidence outbound Palo Alto THREAT events associated with malware communication, DNS tunneling, command-and-control activity, or WildFire detections. The rule focuses on internal hosts generating repeated malicious traffic to multiple external destinations where the firewall did not block the communication. Correlation thresholds are applied to reduce false positives and prioritize potentially compromised systems exhibiting suspicious outbound behavior.
Query:
CommonSecurityLog
| where TimeGenerated >= ago(12h)
| where DeviceVendor == "Palo Alto Networks"
| where DeviceProduct has "PAN-OS"
| where Activity == "THREAT"
| where ipv4_is_private(SourceIP)
| where not(ipv4_is_private(DestinationIP))
| where DeviceAction !in ("deny","drop","reset-both")
| where DeviceEventClassID has_any (
    "DNS Tunnel",
    "Command and Control",
    "WildFire",
    "Malware"
)
| summarize
    EventCount = count(),
    UniqueDestinations = dcount(DestinationIP),
    ThreatNames = make_set(DeviceEventClassID,10),
    DestinationIPs = make_set(DestinationIP,20),
    URLs = make_set(RequestURL,20),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
by SourceIP, DeviceName
| where EventCount >= 10
| where UniqueDestinations >= 4
 
Mitre Attack Mapping:
Tactic	Technique
Command and Control	T1071
Exfiltration	T1041
Defense Evasion	T1001
Ingress Tool Transfer	T1105
77.	 High Privilege User Performing Suspicious Actions

Rule Description
This analytic rule is designed to detect suspicious or threat-related activity involving privileged/service accounts by correlating Palo Alto firewall security events with accounts present in privileged account watchlists. The rule monitors activities such as malware/spyware detections, blocked or denied suspicious traffic, reconnaissance/brute-force events, exploit/phishing detections, and suspicious VPN/security events.
Query:
let TimeWindow = 1h;
let PrivilegedUsers =
union
(
    _GetWatchlist('IT_Service_Accounts')
    | project User=tolower(SearchKey)
),
(
    _GetWatchlist('SEARCH-ServiceAccountDomainAdmin')
    | project User=tolower(SearchKey)
);
CommonSecurityLog
| where TimeGenerated >= ago(TimeWindow)
| where DeviceVendor == "Palo Alto Networks"
| where Activity in ("THREAT","SYSTEM")
    or DeviceAction in ("deny","drop","reset-both")
| extend RawUser=tolower(coalesce(SourceUserName,DestinationUserName))
| where isnotempty(RawUser)
| extend NormalizedUser=
    case(
        RawUser contains "\\", tostring(split(RawUser,"\\")[1]),
        RawUser contains "@", tostring(split(RawUser,"@")[0]),
        RawUser     )
| lookup kind=inner PrivilegedUsers on $left.NormalizedUser == $right.User
| where     DeviceEventCategory has_any ("spyware","virus","malware","exploit","phishing")
    or DeviceEventClassID has_any ("brute-force","scan","flood","overflow","exploit")
    or DeviceAction in ("deny","drop","reset-both")
    or Activity == "THREAT"
| extend SuspiciousActivity =
    case(
        DeviceEventCategory has_any ("spyware","virus","malware"), "Malware Activity",
        DeviceEventCategory has_any ("exploit","phishing"), "Exploit/Phishing Activity",
        DeviceEventClassID has_any ("scan","brute-force","flood"), "Recon/Bruteforce Activity",
        DeviceAction in ("deny","drop"), "Blocked Suspicious Traffic",
        Activity == "THREAT", "Threat Detection",
        "Suspicious Activity"     )
| summarize     EventCount=count(),
    FirstSeen=min(TimeGenerated),
    LastSeen=max(TimeGenerated),
    SourceIPs=make_set(SourceIP,20),
    DestinationIPs=make_set(DestinationIP,20),
    Activities=make_set(SuspiciousActivity,20),
    Devices=make_set(DeviceName,20),
    Actions=make_set(DeviceAction,20)
by NormalizedUser
| where EventCount >= 10 


Mitre Mapping :
Tactics
•	CommandAndControl
•	CredentialAccess
•	DefenseEvasion
•	LateralMovement
•	Exfiltration
Techniques
•	T1071
•	T1071.004
•	T1048
•	T1001
•	T1078
•	T1078.002
•	T1021


Honey account token authentication



Syslog
TACACS+ Brute Force Detection
Syslog
| where Facility == "auth" 
| where ProcessName == "TACACS.net"
| extend User = extract(@"User=(\S+)", 1, SyslogMessage)
| extend NASIP = extract(@"NAS_IP=(\S+)", 1, SyslogMessage)
| extend RemAddr = extract(@"RemAddr=(\S+)", 1, SyslogMessage)
| extend EventId = extract(@"EventId=(\S+)", 1, SyslogMessage)
| summarize 
    AttemptCount = count(),
    UniqueUsers = dcount(User),
    Users = make_set(User),
    NASDevices = make_set(NASIP),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated)
    by RemAddr
| where AttemptCount >= 10 or UniqueUsers >= 5
| extend Severity = iff(AttemptCount >= 20, "High", "Medium")
| project RemAddr, AttemptCount, UniqueUsers, Users, NASDevices, FirstSeen, LastSeen, Severity
 

TACACS+ Authorization Failures Detection
Syslog
| where Facility == "auth" 
| where ProcessName == "TACACS.net"
| where SyslogMessage has_any ("Authorization failed", "Authorization denied", "Authorization reject")
| extend User = extract(@"User=(\S+)", 1, SyslogMessage)
| extend NASIP = extract(@"NAS_IP=(\S+)", 1, SyslogMessage)
| extend RemAddr = extract(@"RemAddr=(\S+)", 1, SyslogMessage)
| extend EventId = extract(@"EventId=(\S+)", 1, SyslogMessage)
| extend ConnectionId = extract(@"ConnectionId=(\S+)", 1, SyslogMessage)
| project TimeGenerated, HostName, User, NASIP, RemAddr, EventId, ConnectionId, SyslogMessage
| summarize FailureCount = count(), FirstFailure = min(TimeGenerated), LastFailure = max(TimeGenerated) by User, NASIP, RemAddr
| where FailureCount >= 3
| extend Severity = "Medium"
 




SSH Suspicious Authentication Activity (Low Threshold)

let threshold = 2;
Syslog
| where ProcessName =~ "sshd"
| where SyslogMessage contains "Failed password for invalid user"
| parse kind=relaxed SyslogMessage with * "invalid user " user " from " ip " port" port " ssh2" *
| summarize 
    AttemptCount = count(), 
    UserCount = dcount(user),
    HostCount = dcount(Computer),
    Users = make_set(user),
    Hosts = make_set(Computer),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated)
by ip, bin(TimeGenerated, 4h)
| where AttemptCount >= threshold 
    or UserCount >= 2 
    or HostCount >= 2
| extend IPAddress = ip
| extend HostName = tostring(Hosts[0])
| extend AccountCustomEntity = tostring(Users[0]) 

SSH - Potential Brute Force

Identifies IP addresses that generate a high number of failed SSH authentication attempts within a 4-hour time window.
The rule triggers when a single IP exceeds 15 failed login attempts, which may indicate a brute force attack. Aggregated results include targeted users and hosts for investigation, with entity mapping based on the first observed values.
•	MITRE ATT&CK Tactic
o	Credential Access
•	MITRE ATT&CK Technique
o	T1110 – Brute Force

let threshold = 15;
Syslog
| where ProcessName =~ "sshd"
| where SyslogMessage contains "Failed password"
| parse kind=relaxed SyslogMessage with * "invalid user " user " from " ip " port" port " ssh2" *
| summarize 
    AttemptCount = count(),
    Users = make_set(user),
    Hosts = make_set(Computer),
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated)
by ip, bin(TimeGenerated, 4h)
| where AttemptCount >= threshold
| extend IPAddress = ip
| extend HostName = tostring(Hosts[0])
| extend AccountCustomEntity = tostring(Users[0]) 

(Preview) Anomalous SSH Login Detection

This detection uses machine learning (ML) to identify anomalous Secure Shell (SSH) login activity, based on syslog data. Scenarios include:
•	Unusual IP - This IP address has not or has rarely been seen in last 30 days.
•	Unusual Geo - The IP address, city, country and ASN have not (or rarely) been seen in last 30 days.
•	New user - A new user logs in from an IP address and geo location, both or either of which are not expected to be seen in the last 30 days.
Allow 7 days after this alert is enabled for Microsoft Sentinel to build a profile of normal activity for your environment.
This detection requires a specific configuration of the data source. Learn more
By enabling this rule, you give Microsoft permission to copy ingested data outside of your Microsoft Sentinel workspace's geography as necessary for processing by the machine learning engine.

Microsoft Defender Threat Intelligence Analytics

This rule generates an alert when a Microsoft Defender Threat Intelligence Indicator gets matched with your event logs. The alerts are very high fidelity.


CyberArk

CyberArk IDP – Privileged Role Creation Detected

Description – 
This analytic rule detects the creation of new roles within CyberArk Identity (IDP).
Role creation is a highly sensitive administrative action that can introduce new privilege structures within the environment.
Attackers who gain access to an administrative account may create new roles to:
•	Establish persistence
•	Escalate privileges
•	Grant unauthorized access to other identities
The rule enriches events using customData to extract role names, session context, authentication method, and actor details, enabling efficient investigation.
Query –
CyberArk_AuditEvents_CL
| where applicationCode == "IDP"
| where action == "Role creation"
| extend Parsed = parse_json(customData)
| extend 
    RoleName = tostring(Parsed.role_name),
    RoleID = tostring(Parsed.role_id),
    TargetUser = tostring(Parsed.user_guid),
    AuthMethod = tostring(Parsed.authentication_method),
    Browser = tostring(Parsed.browser_name),
    Session = tostring(Parsed.session_guid),
    DirectoryService = tostring(Parsed.directory_service_uuid),
    InternalSession = tostring(Parsed.internal_session_id),
    MobileDevice = tostring(Parsed.mobile_device),
    Level = tostring(Parsed.level)
| extend 
    Actor = coalesce(username, userId),
    IPAddress = source
| project 
    TimeGenerated,
    Actor,
    username,
    userId,
    RoleName,
    RoleID,
    TargetUser,
    action,
    AuthMethod,
    Browser,
    IPAddress,
    Session,
    InternalSession,
    DirectoryService,
    MobileDevice,
    Level,
    correlationId,
    serviceName,
    applicationCode,
    Type
| order by TimeGenerated desc

•	Tactic: Privilege Escalation
o	T1078 – Valid Accounts
→ Abuse of legitimate admin access to create roles 
o	T1098 – Account Manipulation
→ Creating roles to modify access control 
•	Tactic: Persistence
o	T1098.003 – Additional Cloud Roles
→ Creating new identity roles for long-term access 
•	Tactic: Defense Evasion
o	T1078 – Valid Accounts
→ Using legitimate identity channels to avoid detection

CyberArk IDP – Non-Admin Role Modification Detected

Description – 
This rule detects role modification activities performed by users who are not part of the approved administrative account list.
Role modification actions such as adding users to roles (AddPrincipal) can grant elevated privileges. When performed by non-administrative users, this may indicate:
•	Privilege escalation
•	Compromised accounts
•	Misconfigured access controls
Query –
CyberArk_AuditEvents_CL
| where applicationCode == "IDP"
| where action == "Role modification"
| extend Parsed = parse_json(customData)
| extend 
    RoleName = tostring(Parsed.role_name),
    RoleID = tostring(Parsed.role_id),
    EditType = tostring(Parsed.edit_type),
    PrincipalName = tostring(Parsed.principal_name),
    PrincipalType = tostring(Parsed.principal_type),
    TargetUser = tostring(Parsed.user_guid),
    AuthMethod = tostring(Parsed.authentication_method),
    Browser = tostring(Parsed.browser_name),
    UserAgent = tostring(Parsed.user_agent),
    Session = tostring(Parsed.session_guid),
    InternalSession = tostring(Parsed.internal_session_id),
    DirectoryService = tostring(Parsed.directory_service_uuid),
    Actor = coalesce(username, userId),
    IPAddress = source
| where Actor !contains "admin"
| project 
    TimeGenerated,
    Actor,
    RoleName,
    EditType,
    PrincipalName,
    PrincipalType,
    AuthMethod,
    Browser,
    IPAddress,
    Session,
    DirectoryService,
    correlationId
| order by TimeGenerated desc
 
CyberArk_AuditEvents_CL
| where applicationCode == "IDP"
| where action == "Role modification"
| extend Parsed = parse_json(customData)
| extend 
    RoleName = tostring(Parsed.role_name),
    PrincipalName = tostring(Parsed.principal_name),
    EditType = tostring(Parsed.edit_type)
| where EditType == "AddPrincipal"
| summarize AddedUsers = count(), Users = make_set(PrincipalName, 20)
    by RoleName, Actor=username, bin(TimeGenerated, 10m)
| where AddedUsers > 1

MITRE ATT&CK Mapping
Privilege Escalation
•	T1098 – Account Manipulation 
Persistence
•	T1098.003 – Additional Cloud Roles 
Defense Evasion
•	T1078 – Valid Accounts
 Security Event

Shell or Script Interpreter Spawned by Suspicious Parent Process

Description – 
This analytic rule detects when a command shell or script interpreter (e.g., cmd.exe, PowerShell, rundll32, mshta) is spawned by a potentially suspicious parent process such as Microsoft Office applications, browsers, WMI, or scripting engines.
Such behavior is commonly associated with:
•	Malicious macros in Office documents 
•	Exploitation of user-facing applications (e.g., browsers, PDF readers) 
•	Living-off-the-Land (LOLBins) abuse 
•	WMI-based remote execution and lateral movement 
The rule includes tuning to reduce false positives by excluding:
•	Legitimate certificate handling via rundll32.exe 
•	Known enterprise automation tools (e.g., Lansweeper) 
•	Common benign WMI activity 
This detection helps identify initial access, execution, and lateral movement techniques used by attackers.
Query –
SecurityEvent
| where EventID == 4688
// Extract executable names
| extend 
    ChildProcess = tolower(tostring(split(NewProcessName, "\\")[-1])),
    ParentProcess = tolower(tostring(split(ParentProcessName, "\\")[-1]))
// Suspicious child processes
| where ChildProcess in~ (
    "cmd.exe", "powershell.exe", "rundll32.exe", "wscript.exe", "cscript.exe",
    "bash.exe", "schtasks.exe", "regsvr32.exe", "hh.exe", "wmic.exe",
    "mshta.exe", "msc.exe", "forfiles.exe", "scriptrunner.exe",
    "mftrace.exe", "appvlp.exe", "svchost.exe", "msbuild.exe"
    )
// Suspicious parent processes
| where ParentProcess matches regex @"^(spoolsv|mshta|wscript|wuapp|taskeng|rundll32|regsvr32|iexplore|winword|powerpnt|excel|access|notepad|outlook|calc|mspub|psexesvc|wsmprovhost|wmiprvse|visio|acrord\d\d|msaccess|eqnedt32)\.exe$"
// Exclusion: Certificate handling
| where not (
            ChildProcess == "rundll32.exe"
    and CommandLine has "cryptext.dll,CryptExtOpenPKCS7"
    and CommandLine has ".p7b"
        )
// Exclusion: Lansweeper
| where not (
            ParentProcess == "wmiprvse.exe"
    and CommandLine has "run_lspush.cmd"
        )
// Exclusion: Common WMI noise
| where not (
            ParentProcess == "wmiprvse.exe"
    and CommandLine has "\\netlogon\\"
        )
// Additional context fields
| extend 
    HostName = tostring(split(Computer, ".")[0]),
    Domain = tostring(split(Computer, ".")[1])
// Final output
| project 
    TimeGenerated,
    Computer,
    HostName,
    Domain,
    Account,
    SubjectUserName,
    SubjectDomainName,
    NewProcessName,
    ParentProcessName,
    ChildProcess,
    ParentProcess,
    CommandLine,
    ProcessId

MITRE ATT&CK Mapping
Tactics
•	Execution (TA0002) 
•	Initial Access (TA0001) 
•	Lateral Movement (TA0008) 
________________________________________
Techniques
•	T1059 – Command and Scripting Interpreter 
•	T1218 – Signed Binary Proxy Execution (LOLBins like rundll32, mshta) 
•	T1047 – Windows Management Instrumentation 
•	T1204 – User Execution 

 
