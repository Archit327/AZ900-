1.	BV - Entra ID - Cross-tenant Access Settings Organization Deleted
Severity – Low
Description
**Summary:**
This Analytic Rule detects the deletion of partner-specific cross-tenant access settings within Azure Active Directory (AAD) audit logs. Such actions may indicate malicious attempts to weaken security controls governing inter-tenant data sharing, potentially enabling unauthorized access or lateral movement between tenants. The rule focuses on deletions of "tenantId" properties within policy resources, which could signify adversarial tampering with access governance.

**Query detail:**
1. Filters `AuditLogs` for operations where `OperationName` contains "Delete partner specific cross-tenant access setting."
2. Extracts the initiator's identity (user principal name or application display name) and IP address.
3. Filters target resources to those of type "Policy."
4. Identifies modified properties with the display name "tenantId" and extracts the deleted external tenant ID from the `oldValue` field.
5. Parses the initiator's user principal name to separate the username and domain suffix.
6. Aggregates results by initiator details, target policy, and deleted tenant ID.

**MITRE Tactics and Techniques:**
- Tactic: TA0003 - Persistence
- Technique: T1098 - Account Manipulation
  Explanation: Deleting cross-tenant access policies may allow adversaries to manipulate account governance rules, potentially maintaining or expanding unauthorized access across tenant boundaries. This aligns with persistence tactics where attackers modify configuration settings to retain access.

**Additional considerations:**
- False positives may occur if legitimate administrators routinely modify cross-tenant access settings as part of normal operations. Contextual analysis of the initiator's role and historical activity is critical.
- Ensure `AuditLogs` are enabled with sufficient retention to capture policy modification events. Gaps in logging will render this rule ineffective.
- Correlate alerts with other anomalous activities, such as unusual login patterns from the initiator's account or concurrent access policy changes.
- Organizations with frequent partner tenant lifecycle changes should implement threshold adjustments or allowlisting to reduce noise.
- Investigate the deleted tenant ID's association with known partners or third parties to assess potential impact.
---

<sub>Description:
This file contains code developed by SNP Cloud Technologies Pvt. Ltd.
Usage Rights:
SNP Cloud Technologies hereby grants the client for which this code was developed a personal, non-exclusive, non-transferable, non-sublicensable license to use the code and to create derivative works of the code, in each case solely for its internal business purposes.
DISCLAIMER:
THIS CODE IS PROVIDED “AS IS” AND WITHOUT WARRANTY OF ANY KIND. SNP Cloud Technologies IS NOT LIABLE FOR ANY DAMAGES ARISING FROM ITS USE.
Copyright (c) SNP Cloud Technologies Pvt. Ltd. All rights reserved.</sub> MITRE: T1098 Category: Entra ID tag: UAS



KQL Query:
let alert_id = "SNP-10001:";
let start_time = 1d;
let end_time = now();
let start_time_suppress = 1d;
 
AuditLogs
| where TimeGenerated >= ago(start_time) and TimeGenerated < end_time
| where OperationName =~ "Delete partner specific cross-tenant access setting"
| extend
    InitiatedByActionUserInformation = iff(
        isnotempty(InitiatedBy.user.userPrincipalName),
        tostring(InitiatedBy.user.userPrincipalName),
        tostring(InitiatedBy.app.displayName)
    ),
    InitiatedByIPAddress = iff(
        isnotempty(InitiatedBy.user.ipAddress),
        tostring(InitiatedBy.user.ipAddress),
        tostring(InitiatedBy.app.ipAddress)
    ),
    InitiatedById = iff(
        isnotempty(InitiatedBy.user.id),
        tostring(InitiatedBy.user.id),
        tostring(InitiatedBy.app.servicePrincipalId)
    )
| mv-apply TargetResource = TargetResources on (
    where TargetResource.type =~ "Policy"
    | extend Properties = TargetResource.modifiedProperties
)
| mv-apply Property = Properties on (
    where Property.displayName =~ "tenantId"
    | extend ExtTenantDeleted = trim(@'"', tostring(Property.oldValue))
)
| extend id_ = tostring(TargetResources[0].id)
| join kind=leftanti (
    SecurityAlert
    | where TimeGenerated >= ago(start_time_suppress)
    | where AlertName has alert_id
    | extend EntitiesParsed = parse_json(tostring(Entities))
    | mv-expand Entity = EntitiesParsed
    | where tostring(Entity.Type) =~ "azure-resource"
    | extend id_ = tostring(Entity.ResourceId)
    | where isnotempty(id_)
    | summarize by id_
) on id_
| extend displayName_ = tostring(TargetResources[0].displayName)
| extend
    Name = tostring(split(InitiatedByActionUserInformation, '@', 0)[0]),
    UPNSuffix = tostring(split(InitiatedByActionUserInformation, '@', 1)[0])
| project-reorder
    TimeGenerated,
    InitiatedById,
    Name,
    UPNSuffix,
    InitiatedByIPAddress,
    ExtTenantDeleted,
    id_,
    displayName_,
    OperationName
 



Rule Importance:
This rule detection means an administrator removed a configured external tenant relationship.
This is important because:
•	B2B collaboration trust may be removed 
•	External partner access may be impacted 
•	An attacker with admin privileges may remove trusted relationships to evade monitoring or disrupt business processes

MITRE Tactics and Techniques:
•	Tactic: TA0003 - Persistence
•	Technique: T1098 - Account Manipulation Explanation: Deleting cross-tenant access policies may allow adversaries to manipulate account governance rules, potentially maintaining or expanding unauthorized access across tenant boundaries. This aligns with persistence tactics where attackers modify configuration settings to retain access.
________________________________________

2.	BV-2105: [DECOM] ASIM Vuln - High Number of Urgent Vulnerabilities Detected
Description
Qualys detected high number of urgent vulnerabilities detected MITRE: T1068,T1189 Category: ASIM,Qualys Vulnerability Management tag: Hygiene tag: DECOM
Severity – High
MITRE ATT&CK 
Privilege Escalation 1- T1068
Initial Access 1 – T1189
Rule query
let start_time = 1d;
let end_time = now();
let alert_bin = 1d;
let threshold = 10;
bv_asim_vuln(ago(start_time), end_time)
| extend EventResultDetails = column_ifexists('EventResultDetails', '')
| extend Status = column_ifexists('Status', '')
| extend Severity = column_ifexists('Vuln_SeverityScore', real(0))
| extend Vuln_Severity = column_ifexists('Vuln_Severity', '')
| extend Vuln_CriticalScore = column_ifexists('Vuln_CriticalScore', real(0))
| extend Vuln_SevereScore = column_ifexists('Vuln_SevereScore', real(0))
| extend TotalVuln = column_ifexists('TotalVuln', '')
| extend Vuln_Same = column_ifexists('Vuln_Same', '')
| extend CVE = column_ifexists('CVE', '')
| extend Status = iff(isnotempty(EventResultDetails), tostring(parse_json(EventResultDetails)[0].Status), Status)
| extend Severity = iff(isnotempty(EventResultDetails), toreal(parse_json(EventResultDetails)[0].Severity), Severity)
| extend Vulnerability = case(isnotempty(EventResultDetails), tostring(parse_json(EventResultDetails)[0]['Results']),
isnotempty(Vuln_Same), extract_all(@'"vulnerability_id": "([\w\d \(\):\.#-0-9_\[\]]*)"', dynamic([1]), Vuln_Same),
isnotempty(CVE), CVE, EventResult)
| where Severity == 5 or Vuln_SevereScore > 0 or Vuln_CriticalScore > 0 or Vuln_Severity in~ ('High', 'Critical')
| summarize StartTime = min(TimeGenerated), EndTime = max(TimeGenerated), VulnCount = count() by DvcHostname, DvcIpAddr, Vulnerability, TotalVuln, EventVendor, bin(TimeGenerated, alert_bin)
| extend VulnCount = iff(isempty(VulnCount), toint(TotalVuln), VulnCount)
| where VulnCount >= threshold
| extend AlertName = strcat('BV-2105: [DECOM] ', EventVendor, ' - High Number of Urgent Vulnerabilities Detected')
| extend Tags = ' tag: Hygiene'
| extend ExtendedDescription = strcat(EventVendor, ' defined a high number of urgent vulnerabilities detected. ', Tags)
| extend SystemAlertPlatform = EventVendor
| project StartTime, EndTime, EventVendor, VulnCount, SystemAlertPlatform, AlertName, Tags, ExtendedDescription, Vulnerability, DvcHostname, DvcIpAddr
 

Rule frequency - Run query every 1 day
Rule period - Last 1 day data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Group all events into a single alert
Suppression - Not configured
________________________________________

3.	BV-70277: [SOC] Entra ID - MFA Spamming Followed by Successful Login
Severity - Low
Description
Summary: This Analytic Rule detects potential account compromise attempts by identifying multiple failed multi-factor authentication (MFA) attempts followed by a successful login within a specified time window where the associated sign-in session has risk signal. The rule analyzes Azure AD sign-in logs to uncover patterns where attackers may be spamming MFA requests to bypass security controls, ultimately gaining unauthorized access to cloud accounts.
Query detail:
1.	Filters SigninLogs for events with AuthenticationRequirement set to "multiFactorAuthentication" within the last 2 hours.
2.	Aggregates data by OriginalRequestId to identify unqiue sign-in attempts and extract the most recent TimeGenerated and contextual details (OS, browser, location).
3.	Parses AuthenticationDetails to categorize MFA results into failed (user declined/no response) and successful attempts.
4.	Summarizes counts of failed and successful MFA attempts per user within 60-minute activity window and 'CorrelationId' to group related sign-ins.
5.	Filters sign-in sessions where risk has been flagged by AAD.
6.	Triggers alerts when a user has ≥1 successful attempt and either >=3 failed attempts in the current window or a cumulative >=5 failures when combined with the prior window.
MITRE Tactics and Techniques:
•	Tactic: TA0001 - Initial Access
o	Technique: T1078 - Valid Accounts (T1078.004 - Cloud Accounts) Explanation: Attackers may spam MFA requests to overwhelm or trick users into approving fraudulent authentications, enabling access to valid cloud accounts. This aligns with the abuse of legitimate credentials (T1078) to gain initial access (TA0001).
Additional considerations:
•	False positives may occur if legitimate users experience repeated MFA failures (e.g., device connectivity issues) before successfully authenticating.
•	Adjust the failedThreshold (10 failures) and period (5 minutes) based on organizational MFA usage patterns to reduce noise.
•	Investigate geographic anomalies (e.g., logins from unusual regions) and device/browser mismatches when validating alerts.
•	Ensure Azure AD sign-in logging and MFA result details are enabled to support this detection.
________________________________________
Description: This file contains code developed by BlueVoyant. Usage Rights: BlueVoyant hereby grants the client for which this code was developed a personal, non-exclusive, non-transferable, non-sublicensable license to use the code and to create derivative works of the code, in each case solely for its internal business purposes. DISCLAIMER: THIS CODE IS PROVIDED “AS IS” AND WITHOUT WARRANTY OF ANY KIND. BLUEVOYANT IS NOT LIABLE FOR ANY DAMAGES ARISING FROM ITS USE. Copyright (c) BlueVoyant. All rights reserved. 

MITRE: 
Initial Access 1 – T1078 – Valid Accounts
Rule query
let failedThreshold = 3; //reduce threshold to detect slow burn attacks and correlate with risk signals
let period = 1h; //increase activity window due to possibility of added social engineering tactic, attacker may contact user posing as IT
let successWindow = 30m;
SigninLogs
| where AuthenticationRequirement == "multiFactorAuthentication"
| summarize arg_max(TimeGenerated,*) by OriginalRequestId, UserId
| extend
    StatusDetails = Status.additionalDetails
    , OS = tostring(DeviceDetail.operatingSystem)
    , Browser = tostring(DeviceDetail.browser)
| summarize
    FailedAttempts = countif(StatusDetails has "MFA denied")
    , SuccessfulAttempts = countif(ResultType == 0)
    , InvolvedOS = make_set(OS, 5)
    , InvolvedBrowser = make_set(Browser)
    , LocationDetails = make_set(LocationDetails)
    , StartTime = min(TimeGenerated)
    , EndTime = max(TimeGenerated)
    , First_Failure = minif(TimeGenerated,StatusDetails has "MFA denied")
    , Last_Failure = maxif(TimeGenerated,StatusDetails has "MFA denied")
    , SuccessSignin = maxif(TimeGenerated,ResultType1.	BV - Entra ID - Cross-tenant Access Settings Organization Deleted
Severity – Low
Description
**Summary:**
This Analytic Rule detects the deletion of partner-specific cross-tenant access settings within Azure Active Directory (AAD) audit logs. Such actions may indicate malicious attempts to weaken security controls governing inter-tenant data sharing, potentially enabling unauthorized access or lateral movement between tenants. The rule focuses on deletions of "tenantId" properties within policy resources, which could signify adversarial tampering with access governance.

**Query detail:**
1. Filters `AuditLogs` for operations where `OperationName` contains "Delete partner specific cross-tenant access setting."
2. Extracts the initiator's identity (user principal name or application display name) and IP address.
3. Filters target resources to those of type "Policy."
4. Identifies modified properties with the display name "tenantId" and extracts the deleted external tenant ID from the `oldValue` field.
5. Parses the initiator's user principal name to separate the username and domain suffix.
6. Aggregates results by initiator details, target policy, and deleted tenant ID.

**MITRE Tactics and Techniques:**
- Tactic: TA0003 - Persistence
- Technique: T1098 - Account Manipulation
  Explanation: Deleting cross-tenant access policies may allow adversaries to manipulate account governance rules, potentially maintaining or expanding unauthorized access across tenant boundaries. This aligns with persistence tactics where attackers modify configuration settings to retain access.

**Additional considerations:**
- False positives may occur if legitimate administrators routinely modify cross-tenant access settings as part of normal operations. Contextual analysis of the initiator's role and historical activity is critical.
- Ensure `AuditLogs` are enabled with sufficient retention to capture policy modification events. Gaps in logging will render this rule ineffective.
- Correlate alerts with other anomalous activities, such as unusual login patterns from the initiator's account or concurrent access policy changes.
- Organizations with frequent partner tenant lifecycle changes should implement threshold adjustments or allowlisting to reduce noise.
- Investigate the deleted tenant ID's association with known partners or third parties to assess potential impact.
---

<sub>Description:
This file contains code developed by SNP Cloud Technologies Pvt. Ltd.
Usage Rights:
SNP Cloud Technologies hereby grants the client for which this code was developed a personal, non-exclusive, non-transferable, non-sublicensable license to use the code and to create derivative works of the code, in each case solely for its internal business purposes.
DISCLAIMER:
THIS CODE IS PROVIDED “AS IS” AND WITHOUT WARRANTY OF ANY KIND. SNP Cloud Technologies IS NOT LIABLE FOR ANY DAMAGES ARISING FROM ITS USE.
Copyright (c) SNP Cloud Technologies Pvt. Ltd. All rights reserved.</sub> MITRE: T1098 Category: Entra ID tag: UAS






KQL Query:
let alert_id = "SNP-10001:";
let start_time = 1d;
let end_time = now();
let start_time_suppress = 1d;
 
AuditLogs
| where TimeGenerated >= ago(start_time) and TimeGenerated < end_time
| where OperationName =~ "Delete partner specific cross-tenant access setting"
| extend
    InitiatedByActionUserInformation = iff(
        isnotempty(InitiatedBy.user.userPrincipalName),
        tostring(InitiatedBy.user.userPrincipalName),
        tostring(InitiatedBy.app.displayName)
    ),
    InitiatedByIPAddress = iff(
        isnotempty(InitiatedBy.user.ipAddress),
        tostring(InitiatedBy.user.ipAddress),
        tostring(InitiatedBy.app.ipAddress)
    ),
    InitiatedById = iff(
        isnotempty(InitiatedBy.user.id),
        tostring(InitiatedBy.user.id),
        tostring(InitiatedBy.app.servicePrincipalId)
    )
| mv-apply TargetResource = TargetResources on (
    where TargetResource.type =~ "Policy"
    | extend Properties = TargetResource.modifiedProperties
)
| mv-apply Property = Properties on (
    where Property.displayName =~ "tenantId"
    | extend ExtTenantDeleted = trim(@'"', tostring(Property.oldValue))
)
| extend id_ = tostring(TargetResources[0].id)
| join kind=leftanti (
    SecurityAlert
    | where TimeGenerated >= ago(start_time_suppress)
    | where AlertName has alert_id
    | extend EntitiesParsed = parse_json(tostring(Entities))
    | mv-expand Entity = EntitiesParsed
    | where tostring(Entity.Type) =~ "azure-resource"
    | extend id_ = tostring(Entity.ResourceId)
    | where isnotempty(id_)
    | summarize by id_
) on id_
| extend displayName_ = tostring(TargetResources[0].displayName)
| extend
    Name = tostring(split(InitiatedByActionUserInformation, '@', 0)[0]),
    UPNSuffix = tostring(split(InitiatedByActionUserInformation, '@', 1)[0])
| project-reorder
    TimeGenerated,
    InitiatedById,
    Name,
    UPNSuffix,
    InitiatedByIPAddress,
    ExtTenantDeleted,
    id_,
    displayName_,
    OperationName
 



Rule Importance:
This rule detection means an administrator removed a configured external tenant relationship.
This is important because:
•	B2B collaboration trust may be removed 
•	External partner access may be impacted 
•	An attacker with admin privileges may remove trusted relationships to evade monitoring or disrupt business processes

MITRE Tactics and Techniques:
•	Tactic: TA0003 - Persistence
•	Technique: T1098 - Account Manipulation Explanation: Deleting cross-tenant access policies may allow adversaries to manipulate account governance rules, potentially maintaining or expanding unauthorized access across tenant boundaries. This aligns with persistence tactics where attackers modify configuration settings to retain access.
________________________________________
2.	BV-2105: [DECOM] ASIM Vuln - High Number of Urgent Vulnerabilities Detected
Description
Qualys detected high number of urgent vulnerabilities detected MITRE: T1068,T1189 Category: ASIM,Qualys Vulnerability Management tag: Hygiene tag: DECOM
Severity – High
MITRE ATT&CK 
Privilege Escalation 1- T1068
Initial Access 1 – T1189
Rule query
let start_time = 1d;
let end_time = now();
let alert_bin = 1d;
let threshold = 10;
bv_asim_vuln(ago(start_time), end_time)
| extend EventResultDetails = column_ifexists('EventResultDetails', '')
| extend Status = column_ifexists('Status', '')
| extend Severity = column_ifexists('Vuln_SeverityScore', real(0))
| extend Vuln_Severity = column_ifexists('Vuln_Severity', '')
| extend Vuln_CriticalScore = column_ifexists('Vuln_CriticalScore', real(0))
| extend Vuln_SevereScore = column_ifexists('Vuln_SevereScore', real(0))
| extend TotalVuln = column_ifexists('TotalVuln', '')
| extend Vuln_Same = column_ifexists('Vuln_Same', '')
| extend CVE = column_ifexists('CVE', '')
| extend Status = iff(isnotempty(EventResultDetails), tostring(parse_json(EventResultDetails)[0].Status), Status)
| extend Severity = iff(isnotempty(EventResultDetails), toreal(parse_json(EventResultDetails)[0].Severity), Severity)
| extend Vulnerability = case(isnotempty(EventResultDetails), tostring(parse_json(EventResultDetails)[0]['Results']),
isnotempty(Vuln_Same), extract_all(@'"vulnerability_id": "([\w\d \(\):\.#-0-9_\[\]]*)"', dynamic([1]), Vuln_Same),
isnotempty(CVE), CVE, EventResult)
| where Severity == 5 or Vuln_SevereScore > 0 or Vuln_CriticalScore > 0 or Vuln_Severity in~ ('High', 'Critical')
| summarize StartTime = min(TimeGenerated), EndTime = max(TimeGenerated), VulnCount = count() by DvcHostname, DvcIpAddr, Vulnerability, TotalVuln, EventVendor, bin(TimeGenerated, alert_bin)
| extend VulnCount = iff(isempty(VulnCount), toint(TotalVuln), VulnCount)
| where VulnCount >= threshold
| extend AlertName = strcat('BV-2105: [DECOM] ', EventVendor, ' - High Number of Urgent Vulnerabilities Detected')
| extend Tags = ' tag: Hygiene'
| extend ExtendedDescription = strcat(EventVendor, ' defined a high number of urgent vulnerabilities detected. ', Tags)
| extend SystemAlertPlatform = EventVendor
| project StartTime, EndTime, EventVendor, VulnCount, SystemAlertPlatform, AlertName, Tags, ExtendedDescription, Vulnerability, DvcHostname, DvcIpAddr
 

Rule frequency - Run query every 1 day
Rule period - Last 1 day data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Group all events into a single alert
Suppression - Not configured
________________________________________
3.	BV-70277: [SOC] Entra ID - MFA Spamming Followed by Successful Login
Severity - Low
Description
Summary: This Analytic Rule detects potential account compromise attempts by identifying multiple failed multi-factor authentication (MFA) attempts followed by a successful login within a specified time window where the associated sign-in session has risk signal. The rule analyzes Azure AD sign-in logs to uncover patterns where attackers may be spamming MFA requests to bypass security controls, ultimately gaining unauthorized access to cloud accounts.
Query detail:
1.	Filters SigninLogs for events with AuthenticationRequirement set to "multiFactorAuthentication" within the last 2 hours.
2.	Aggregates data by OriginalRequestId to identify unqiue sign-in attempts and extract the most recent TimeGenerated and contextual details (OS, browser, location).
3.	Parses AuthenticationDetails to categorize MFA results into failed (user declined/no response) and successful attempts.
4.	Summarizes counts of failed and successful MFA attempts per user within 60-minute activity window and 'CorrelationId' to group related sign-ins.
5.	Filters sign-in sessions where risk has been flagged by AAD.
6.	Triggers alerts when a user has ≥1 successful attempt and either >=3 failed attempts in the current window or a cumulative >=5 failures when combined with the prior window.
MITRE Tactics and Techniques:
•	Tactic: TA0001 - Initial Access
o	Technique: T1078 - Valid Accounts (T1078.004 - Cloud Accounts) Explanation: Attackers may spam MFA requests to overwhelm or trick users into approving fraudulent authentications, enabling access to valid cloud accounts. This aligns with the abuse of legitimate credentials (T1078) to gain initial access (TA0001).
Additional considerations:
•	False positives may occur if legitimate users experience repeated MFA failures (e.g., device connectivity issues) before successfully authenticating.
•	Adjust the failedThreshold (10 failures) and period (5 minutes) based on organizational MFA usage patterns to reduce noise.
•	Investigate geographic anomalies (e.g., logins from unusual regions) and device/browser mismatches when validating alerts.
•	Ensure Azure AD sign-in logging and MFA result details are enabled to support this detection.
________________________________________
Description: This file contains code developed by BlueVoyant. Usage Rights: BlueVoyant hereby grants the client for which this code was developed a personal, non-exclusive, non-transferable, non-sublicensable license to use the code and to create derivative works of the code, in each case solely for its internal business purposes. DISCLAIMER: THIS CODE IS PROVIDED “AS IS” AND WITHOUT WARRANTY OF ANY KIND. BLUEVOYANT IS NOT LIABLE FOR ANY DAMAGES ARISING FROM ITS USE. Copyright (c) BlueVoyant. All rights reserved. 

MITRE: 
Initial Access 1 – T1078 – Valid Accounts
Rule query
let failedThreshold = 3; //reduce threshold to detect slow burn attacks and correlate with risk signals
let period = 1h; //increase activity window due to possibility of added social engineering tactic, attacker may contact user posing as IT
let successWindow = 30m;
SigninLogs
| where AuthenticationRequirement == "multiFactorAuthentication"
| summarize arg_max(TimeGenerated,*) by OriginalRequestId, UserId
| extend
    StatusDetails = Status.additionalDetails
    , OS = tostring(DeviceDetail.operatingSystem)
    , Browser = tostring(DeviceDetail.browser)
| summarize
    FailedAttempts = countif(StatusDetails has "MFA denied")
    , SuccessfulAttempts = countif(ResultType == 0)
    , InvolvedOS = make_set(OS, 5)
    , InvolvedBrowser = make_set(Browser)
    , LocationDetails = make_set(LocationDetails)
    , StartTime = min(TimeGenerated)
    , EndTime = max(TimeGenerated)
    , First_Failure = minif(TimeGenerated,StatusDetails has "MFA denied")
    , Last_Failure = maxif(TimeGenerated,StatusDetails has "MFA denied")
    , SuccessSignin = maxif(TimeGenerated,ResultType== 0)
    , RiskEventTypes_V2 = make_set_if(RiskEventTypes_V2,RiskEventTypes_V2!="[]")
    , OriginalRequestIds = make_set(OriginalRequestId)
by
    UserId
    , UserPrincipalName
    , ActivityWindow = bin(TimeGenerated, period)
    , CorrelationId
    , IPAddress
| where array_length(todynamic(RiskEventTypes_V2)) != 0
| sort by
    UserId
    , StartTime asc
| where SuccessfulAttempts >= 1
    and (
        FailedAttempts >= failedThreshold
        or (
            (FailedAttempts + prev(FailedAttempts)) >= failedThreshold*1.5
            and prev(UserId) == UserId
        )
    )
| where SuccessSignin between (First_Failure .. (Last_Failure + successWindow))
 

Rule frequency - Run query every 15 minutes
Rule period - Last 2 hours data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Enabled
Alert grouping - Enabled
Grouping logic - Match selected entity types and details
Grouping period - Match from the last 8 hours

 

4.	BV-72006: [SOC] Entra ID - Exchange Online Reconnaissance via Microsoft Graph Search Queries
Severity - Medium
Description
Summary: This Analytic Rule detects suspicious reconnaissance activity in Exchange Online and Entra ID environments by identifying abnormal Microsoft Graph GET requests that leverage advanced $search queries. Attackers often perform reconnaissance after initial access to enumerate users, roles, group membership, or sensitive mailbox content. This rule correlates Graph search activity targeting high-value business terms (e.g., payroll, finance, HR, salary) with User and Entity Behavior Analytics (UEBA) anomalies, highlighting behavior consistent with discovery and targeting prior to data theft or further compromise.
Query detail:
1.	Collects UEBA insights from BehaviorAnalytics for Azure AD activity within a 2-hour window, focusing on anomalous sign-in characteristics such as uncommon ISPs, browsers, countries, or applications.
2.	Aggregates UEBA signals by user ID and source IP, summarizing first observed anomaly time and insight categories.
3.	Monitors Microsoft Graph activity from MicrosoftGraphActivityLogs over the last hour.
4.	Filters for successful GET requests (ResponseStatusCode < 400) using the $search parameter in Graph queries.
5.	Scopes queries to reconnaissance-relevant endpoints, including directory roles, group members, users, and mailbox messages.
6.	Identifies high-risk search intent by matching multiple sensitive business keywords (support, payroll, HR, finance, salary, account, info) appearing at least three times within the same request.
7.	Applies request complexity heuristics, requiring long request URIs (≥500 characters) and non-zero response sizes, indicating bulk or structured enumeration.
8.	Correlates Graph activity with UEBA anomalies within a ±5-hour window to increase confidence in malicious intent.
9.	Enriches with sign-in context by joining recent successful authentication events to associate sessions with user identities.
10.	Summarizes activity per user, capturing event count, time range, source IPs, and request characteristics.
11.	Deduplicates alerts by excluding users previously alerted by this rule (BV-72006) within the last 24 hours.
MITRE Tactics and Techniques:
•	Tactic: TA0007 - Discovery
o	T1087.004 - Account Discovery: Cloud Account:
Graph queries targeting /users and role membership endpoints indicate enumeration of cloud identities.
o	T1069.003 - Permission Groups Discovery: Cloud Groups:
Accessing directory roles and group membership endpoints supports discovery of privileged users and roles.
o	T1114.002 - Email Collection: Remote Email Collection:
Graph queries against /messages may be used to identify sensitive or high-value email content.
•	Tactic: TA0001 - Initial Access
o	T1078.004 - Valid Accounts: Cloud Accounts:
Reconnaissance is performed using authenticated sessions from compromised or misused cloud accounts.
•	Tactic: TA0005 - Defense Evasion
o	T1071.001 - Application Layer Protocol: Web Protocols:
Microsoft Graph over HTTPS is abused to blend reconnaissance activity with legitimate API traffic.
Additional considerations:
•	False Positives:
Legitimate eDiscovery operations, compliance audits, SOC threat-hunting, automated governance tooling, or authorized administrative scripts using Microsoft Graph search.
•	Investigation Guidance:
Validate whether the user is authorized to perform bulk search operations. Review the volume and structure of Graph queries, focusing on repeated sensitive keywords. Examine source IPs and locations for anomalies. Correlate with UEBA investigation priority and recent authentication events. Review app permissions if activity was performed via delegated or application access.
•	Response Recommendations:
If malicious activity is confirmed, suspend or restrict the affected account, revoke active sessions, review application consent grants, and audit access to sensitive mailboxes or directory objects. Implement least-privilege Graph permissions and monitor for follow-on activity such as data exfiltration.
________________________________________
Description: This file contains code developed by BlueVoyant. Usage Rights: BlueVoyant hereby grants the client for which this code was developed a personal, non-exclusive, non-transferable, non-sublicensable license to use the code and to create derivative works of the code, in each case solely for its internal business purposes. DISCLAIMER: THIS CODE IS PROVIDED “AS IS” AND WITHOUT WARRANTY OF ANY KIND. BLUEVOYANT IS NOT LIABLE FOR ANY DAMAGES ARISING FROM ITS USE. Copyright (c) BlueVoyant. All rights reserved. 

MITRE Attack : T1087,T1069,T1114,T1078,T1071 
Category: Entra ID tag: SOC
Rule query
let alert_id = "BV-72006:";
let end_time = now();
let start_time = 1h;
let start_time_suppress = 1d;
let UEBA_Activity =
    union isfuzzy = true
    (
        datatable(
            TimeGenerated:datetime,
            ActivityInsights:dynamic,
            InvestigationPriority:int,
            InitiatingAadUserId:string,
            SourceIPAddress:string,
            SourceIPLocation:string,
            ActionType:string,
            ActivityType:string,
            UserPrincipalName:string
        )[]
    ),
    (
        BehaviorAnalytics
        | where TimeGenerated >= ago(start_time + start_time)
        | where EventSource == "Azure AD"
        | extend
            InitiatingAadUserId = tostring(UsersInsights.AccountObjectID)
    )
    | project
        UEBA_Time                      = TimeGenerated,
        ActivityInsights,
        InvestigationPriority,
        InitiatingAadUserId,
        InitiatingIpAddress            = SourceIPAddress,
        SourceIPLocation,
        ActionType,
        ActivityType,
        InitiatingAadUserPrincipalName = tolower(UserPrincipalName)
    | where
        ActivityInsights.CountryUncommonlyConnectedFromByUser == true
        or ActivityInsights.ISPUncommonlyUsedByUser == true
        or ActivityInsights.ISPUncommonlyUsedInTenant == true
        or ActivityInsights.FirstTimeUserConnectedViaBrowser == true
        or ActivityInsights.BrowserUncommonlyUsedAmongPeers == true
        or ActivityInsights.FirstTimeUserUsedApp == true
        or ActivityInsights.AppUncommonlyUsedAmongPeers == true
    | summarize
        FirstUEBATime = arg_min(UEBA_Time, *),
        InsightSummary = make_set(
            extract_all('"([^",]+)":"True"', tostring(ActivityInsights)),
            10
        )
      by InitiatingAadUserId, InitiatingIpAddress, InitiatingAadUserPrincipalName
    | project-away ActivityInsights;
MicrosoftGraphActivityLogs
| where TimeGenerated >= ago(start_time)
| where RequestMethod == "GET"
| where toint(ResponseStatusCode) < 400
| where RequestUri has_all ("https://graph.microsoft.com/", "users?$search=")
| where RequestUri has_any ("/directoryRoles/", "members/$ref", "/users", "/messages")
| where tolower(RequestUri) matches regex "support%|pay%|payroll%|hr%|human%|account%|info%|finance%|salary%"
| where
    array_length(
        extract_all(
            @"(?i)(support%|pay%|payroll%|hr%|human%|account%|info%|finance%|salary%)",
            tolower(RequestUri)
        )
    ) >= 3
| extend
    RequestUri_Length = strlen(RequestUri)
| where RequestUri_Length >= 500
| where ResponseSizeBytes > 0
| lookup kind = inner (UEBA_Activity) on $left.UserId == $right.InitiatingAadUserId
| where FirstUEBATime - TimeGenerated between (-5h .. 5h)
| lookup kind = leftouter
(
    SigninLogs
    | where TimeGenerated >= ago(start_time + start_time)
    | where ResultType == 0
    | summarize by SessionId, UserPrincipalName, UserId
) on SessionId, UserId
| summarize
    StartTime  = min(TimeGenerated),
    EndTime    = arg_max(TimeGenerated, *),
    EventCount = count(),
    IPs        = make_set(IPAddress, 10)
  by UserId
| join kind = leftanti
(
    SecurityAlert
    | where TimeGenerated >= ago(start_time_suppress)
    | where AlertName has alert_id
    | mv-apply e = parse_json(Entities) on
    (
        where e.Type has "account"
        | extend UserId = tostring(e.AadUserId)
        | where isnotempty(UserId)
        | summarize by UserId
    )
) on UserId
| project-away *1
| extend
    UserName  = tostring(split(UserPrincipalName, "@", 0)[0]),
    UPNSuffix = tostring(split(UserPrincipalName, "@", 1)[0])
| project-reorder
    StartTime,
    EndTime,
    UserId,
    UserPrincipalName,
    UserAgent,
    IPAddress,
    EventCount,
    IPs,
    FirstUEBATime,
    InsightSummary,
    DurationMs,
    ResponseSizeBytes,
    RequestUri,
    RequestUri_Length
 

Rule frequency - Run query every 1 hour
Rule period - Last 1 day data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Enabled
Alert grouping - Enabled
Grouping logic - Match all entities
Grouping period - Match from the last 1 day
________________________________________
5.	BV-030: [RBA] Azure AD - Conditional Access Policy Modified
Severity - LOW
Description
An Azure AD conditional access policy has been modified MITRE: T1556 Category: Entra ID tag: RBA
MITRE ATT&CK 
Defense Evasion – 1 – T1556
Credential Access – 1 – T1556
Persistence – 1 -  T1556
Rule query
let ingested_lookback = 5min; // earliest [ingest time] boundary
let generated_lookback = 1h; // earliest [log time] boundary
AuditLogs
| where TimeGenerated > ago(generated_lookback) and ingestion_time() > ago(ingested_lookback)
| where ActivityDisplayName == 'Update policy'
| extend ParsedInitiatedBy = parse_json(InitiatedBy)
| extend AccountCustomEntity = iff(notnull(ParsedInitiatedBy.user.userPrincipalName),tostring(ParsedInitiatedBy.user.userPrincipalName), tostring(ParsedInitiatedBy.app.displayName))
| project TimeGenerated, AccountCustomEntity, Result, Policy=tostring(TargetResources[0].displayName), TargetResources, AADOperationType, _ItemId
| summarize EventCount=count(), Events=make_set(pack_all()), Polices=make_set(tostring(TargetResources[0].displayName)) by AccountCustomEntity, Policy, bin(TimeGenerated, 1h)
 

Rule frequency - Run query every 1 hour
Rule period - Last 1 hour data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Disabled
Alert grouping – Disabled
________________________________________


6.	BV-011: [RBA] O365 - Anonymous Sharepoint Link accessed
Severity – Information
Description
This alert detects when an anonymous link created in Sharepoint has been used. The anonymous link allow access to the shared document without any credentials. MITRE: T1213 Category: DefenderXDR/Microsoft Defender XDR tag: RBA
MITRE ATT&CK
Collection 1 – T1213 data from information repositories
Rule query
let ingested_lookback = 1d; // earliest [ingest time] boundary
let generated_lookback = 3d; // earliest [log time] boundary
OfficeActivity
| where TimeGenerated > ago(generated_lookback) and ingestion_time() > ago(ingested_lookback)
| where Operation =~ 'AnonymousLinkUsed'
| project TimeGenerated, RecordType, SourceRelativeUrl, Site_Url, IPCustomEntity=ClientIP, UserAgent
| summarize CountofEvents= count(), starttime=min(TimeGenerated), endtime=max(TimeGenerated), IPList = make_set(IPCustomEntity), RecordTypes = make_set(RecordType), SourceRelativeUrls = make_set(SourceRelativeUrl), Site_Urls = make_set(Site_Url) by UserAgent, bin(TimeGenerated, 1d)
| sort by CountofEvents desc
 

Rule frequency - Run query every 1 day
Rule period - Last 3 days data - Rule threshold
Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Disabled
Alert grouping – Disabled
________________________________________
7.	BV-027: [RBA] Linux - Servers with Authentication Exposed to Public Internet
Severity – Medium 
Description
This alert identifies Linux servers that appear to be accessible from public IP addresses (for authentication). MITRE: T1133 Category: Windows/Linux Machines tag: RBA
MITRE ATT&CK
Persistence 1 – T1133
Initial Access 1 – T1133
Rule query
let time_bin = 1h;
let ingested_lookback = 1h; // earliest [ingest time] boundary
let generated_lookback = 3h; // earliest [log time] boundary
let alert_id = 'BV-027:';
let start_time_suppress = 3d;
let PreviousAlerts = toscalar(SecurityAlert
|where TimeGenerated between (ago(A027_SUPPRESSION_THRESHOLD) .. now())
|where AlertName has alert_id
| mv-expand parse_json(Entities)
| extend IPAddress = iff(Entities.Type =~ 'ip', Entities.Address, '')
| summarize make_set_if(IPAddress, isnotempty(IPAddress)));
union (Syslog
| where Facility in~ ('auth','authpriv')
| where TimeGenerated between ( ago(generated_lookback) .. now()) and ingestion_time() > ago(ingested_lookback)
| where SyslogMessage startswith 'pam_unix(sshd:auth): authentication failure'
| extend UserName = extract(' user=(\\S+)',1,SyslogMessage)
| extend SourceIP = extract(' rhost=(\\S+)',1,SyslogMessage)
| summarize maxFailTime=max(TimeGenerated), FailedLogins=count() by UserName, SourceIP, TargetIP=HostIP, Computer, bin(TimeGenerated, time_bin)),
(Syslog
| where Facility in~ ('auth','authpriv')
| where TimeGenerated between ( ago(generated_lookback) .. now()) and ingestion_time() > ago(ingested_lookback)
| where SyslogMessage startswith 'Failed password for invalid user'
| extend UserName = extract('Failed password for invalid user (\\S+)',1,SyslogMessage)
| extend SourceIP = extract('Failed password for invalid user (\\S+) from (\\S+) port',2,SyslogMessage)
| summarize maxFailTime=max(TimeGenerated), FailedLogins=count() by UserName, SourceIP, TargetIP=HostIP, Computer, bin(TimeGenerated, time_bin)),
(Syslog
| where Facility in~ ('auth','authpriv')
| where TimeGenerated between ( ago(generated_lookback) .. now()) and ingestion_time() > ago(ingested_lookback)
| where SyslogMessage contains 'Accepted password'
| parse SyslogMessage with * 'Accepted password for ' UserName ' from ' SourceIP ' ' *
| summarize minSuccessTime=min(TimeGenerated), SuccessfulLogins=count() by UserName, SourceIP, TargetIP=HostIP, Computer, bin(TimeGenerated, time_bin)
)
| mv-apply l=A027_EXCLUDED_SOURCE_IP_RANGES to typeof(string) on //Not in IP list
(
extend ExcludedSourceIP = ipv4_is_match(l,SourceIP)
)
| where isnotempty(UserName) and isnotempty(SourceIP)
| where ipv4_is_private(SourceIP) == false
| where UserName in~ (A027_UNIQUE_ACCOUNT_RARELY_USED)
//| summarize FailedLogins = sum(FailedLogins), SuccessfulLogins = sum(SuccessfulLogins), SourceIP = make_set(SourceIP, 5), NumberOfSourceIPs = dcount(SourceIP) 
//            by TargetUser=UserName, TargetIP, LogSourceHost = Computer, ExcludedSourceIP, minSuccessTime, maxFailTime, bin(TimeGenerated, time_bin)
| summarize FailedLogins = sum(FailedLogins), SuccessfulLogins = sum(SuccessfulLogins), SourceIP = make_set(SourceIP,5), NumberOfSourceIPs = dcount(SourceIP) 
            by TargetUser=UserName, TargetIP, LogSourceHost = Computer, ExcludedSourceIP, bin(TimeGenerated, time_bin)
| extend bv_dedup_hash = hash_sha1(strcat(TargetIP))
| where BV_FUN_DEDUP(alert_id, start_time_suppress) !has bv_dedup_hash  
| where FailedLogins >= A027_FAILURE_THRESHOLD
| extend NonSuppressedEvent = iff(TargetIP !in (PreviousAlerts), true, false)
//| extend SuccessfulEvents = iff(SuccessfulLogins > 0 and minSuccessTime > maxFailTime, true, false)
| extend SuccessfulEvents = iff(SuccessfulLogins > 0, true, false)
| where not(SuccessfulEvents and ExcludedSourceIP and NonSuppressedEvent)
| extend Class = 'RBA'
, Tags = 'tag: RBA'
, Severity = 'Low'
| extend AlertName = strcat('BV-027: [', Class, '] Linux servers with authentication exposed to Internet')
| extend Description = 'This alert identify Linux servers that appear to be accessible from public IP addresses (for authentication)'
| extend Description = strcat(Description, ' MITRE: T1110 Category: Linux ', Tags)
| extend SystemAlertPlatform = 'Correlation'
| project-away ExcludedSourceIP, SuccessfulEvents
 

Rule frequency - Run query every 1 hour
Rule period - Last 3 days data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Disabled
Alert grouping – Disabled
________________________________________
8.	BV-032: [RBA] Windows - Multiple Password Resets
Severity – Low
Description
This alert identifies Windows accounts that attempted 5 or more password resets in 24 hours. MITRE: T1098 Category: Windows Security Events tag: RBA
MITRE ATT&CK
Persistence 1 – T1098
Rule query
let EventIDList = dynamic(['4723', '4724']);
let ingested_lookback = 1d;
let generated_lookback = 2d;
let alert_bin=1d;
SecurityEvent
| where TimeGenerated between ( ago(generated_lookback) .. now()) and ingestion_time() > ago(ingested_lookback)
| where EventID in (EventIDList)
| where AccountType == 'User'
| where tolower(Account) matches regex tolower(EXCLUDED_USER_ACCOUNTS) == false
| where tolower(TargetAccount) matches regex tolower(A032_EXCLUDED_TARGET_ACCOUNT) == false
| project TimeGenerated, Computer, Account, Activity, TargetAccount, SubjectUserSid, SubjectUserName, _ItemId
| summarize Events = make_set(pack_all()), EventCount = count(), TargetAccounts = make_set(TargetAccount), NumberUniqueUsers=dcount(TargetAccount), ItemIds=make_set(_ItemId) by  SubjectUserSid, SubjectUserName, Computer, bin(TimeGenerated, alert_bin)
| where EventCount >= PASSWORD_RESETS_THRESHOLD
| extend TargetAccounts=tostring(TargetAccounts)
 

Rule frequency - Run query every 1 day
Rule period - Last 2 days data
Rule threshold - Trigger alert if query returns more than 0 results
Event grouping - Trigger an alert for each event
Suppression - Not configured
Create incidents from this rule - Disabled
Alert grouping - Disabled

