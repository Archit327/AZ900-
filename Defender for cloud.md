
What is a defender for cloud ?
Why DFC?
need for DFC?
What is XDR?
features of DFC?


Cloud security explorer
it equips security admins and power users with query based tools and starter templates for risk hunting and resource exploring , enabling user to query the graph for their own custom findings.


Defender for cloud really is something that we call a synapse solution.

And cnap is a terme that was defined by Gartner and just stands for Cloud Native Application Protection Platform


Cloud security posture management

So what is cloud security posture management.
One is cloud security posture management and the other is cloud workload protection.
Now cloud security posture management or Cspm, most of the times is a proactive and preventive way of doing security instead of waiting for threats.

Cspm is all around being proactive and trying to harden the resources that you control instead of waiting for incidents to occur.

---

---
title: Udemy
date: 2024-01-30
resources: 
tags:
---

# Introduction

- CNAP solution - Cloud Native Application Protection.

![[Pasted image 20240130201331.png]]

- Scan the repos of github in *DevSecOps*.
- Misconfigs in the code.
- Misconfigs of vms
<br>
- *CSPM* gives the recommendations in a proactive way and preventative way before anything bad happens.
<br>
- *CWPP* deals with runtime protection for various resources and deals with actual threats, such as:
	- App Service
	- Databases
	- DB
	- Storage
	- Containers
	- Key Vault
	- Resource Manager
	- APIs

- All features are ava in all the cloud platforms
- Protecting from the first line of code (devSecOps) to various resources (CWPP).

## CSPM Vs CWPP

- Ava plans of CSPM and CWPP.
- ! DNS for DNS and Defender for kubernetes, those are been deprecated.
- Plans:
- ![[Pasted image 20240131104515.png | 300]]
- Defender also comes with *RBAC* model.
- Pre-built RBAC roles for Defender for cloud.


| Role | Add/assign initiatives (including regulatory compliance standards) | Edit security policy | Enable/disable Microsoft Defender plans | Suppress alerts | Apply security recommendations for a resource | View alerts and recommendations |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Security Reader | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Security Admin | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| Contributor/Owner (RG Level) | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| Contributor (Subscription Level) | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Owner (Subscription Level) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---
## Azure Policy

- Defines a desired configs of an Azure resource.
- DFC utilizes Azure Policy for all security recommendations.
- Azure Policy examples in the context of security:
	- Enforce the deployment of defender for servers for all VMS
	- Audit whether an NSG is associated with a subnet
	- Deny resource creation if vulnerabilities are identified

### Effects in Policy

- These effects are currently supportedin a policy definition:
	- AddToNetworkGroup
	- Append
	- Audit - *allows to do things but with an alert*
	- AuditlfNotExists
	- Deny - *deny the things you want to do*
	- DenyAction
	- DeploylfNotExists - *deploy the resource if certain properties are not fulfilled *
		- Eg - deploy *defender for servers* when you create a VM.
	- Disabled
	- Manual
	- Modify
	- Mutate

```cardlink
url: https://learn.microsoft.com/en-us/azure/governance/policy/concepts/effects
title: "Understand how effects work - Azure Policy"
description: "Azure Policy definitions have various effects that determine how compliance is managed and reported."
host: learn.microsoft.com
image: https://learn.microsoft.com/en-us/media/open-graph-image.png
```

- #Azure -policy is heavily tied to #ARM, works together.
- ![[Pasted image 20240131112542.png]]
- ![[Pasted image 20240131112925.png | 500]]

---
## ARC

Other Sources - [[ARC]]

- Manage VMs, Kubernetes Clusters and databases as If they run in Azure
- Creates an object in Azure That allows interaction with The Azure Resource Manager

![[Pasted image 20240131113300.png | 400]]

## KQL Query Development Process
#KQL-Query
1. Hypothesis
	- A query always begins with a hypothesis that you want to prove or disprove,
	- Such as: Is this IoC part of my logs?
2. Determine required tables
	- Determine the required tables for your query
3. Explore table schema
	- Consider the schema
4. Filter away
5. Visualize

#KQL/Kusto-Query-Language

![[Pasted image 20240131114027.png | 500]]

- All connected data sources are ingested in specific tables

![[Pasted image 20240131114100.png]]

![[Pasted image 20240131114135.png | 500]]

KQL-Important Operators

```cardlink
url: https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/kql-quick-reference#:~:text=KQL%20quick%20reference,-Article
title: "KQL quick reference - Azure Data Explorer & Real-Time Analytics"
description: "A list of useful KQL functions and their definitions with syntax examples."
host: learn.microsoft.com
image: https://learn.microsoft.com/en-us/media/open-graph-image.png
```

---
## Log Analytics Dedicated Cluster

#Log-Analytics supports a premium #dedicated-cluster option with the
Following features:
- Customer #Lockbox Double encryption of data at rest Support for Availability Zones
- Increased performance for cross-workspace queries
- Minimum commitment: 500 GB per day.

---
# CSPM

- Proactive and preventive security instead of waiting for threats
- Detailed visibility into the security state of your assets and workloads
- Hardening guidance to help you efficiently and effectively improve your Security posture

# CSPM Plans

![[Pasted image 20240131124030.png | 400]]

![[Pasted image 20240131124042.png | 400]]

![[Pasted image 20240131124203.png | 400]]

---
## Foundational CSPM

### Inventory

- Inventory of all Azure resources connected to Defender for Cloud
- Can be leveraged to answer questions such as:
	- Which of my subscriptions have outstanding recommendations?
	- Which of my machines with the tag 'Critical' are missing the Azure Monitor Agent?
	- Which machines in a specific resource group have a known vulnerability (using A CVE number)?

- Features:
	- Filtering the resources, based on recommendations, ...
	- Total resources
	- Unhealthy resources
	- Unmonitored resources
	- Unreg subs
	- Can add non-azure resources
	- Open Query and interact using KQL query
	- Download as CSV report
	- Recommendations for a specific resource
	- Monitoring agent installed or not
	- Defender for cloud is On or Off for a resource.

---
### Security Recommendations

- Defender for Cloud assesses your resources against security Controls in Azure, AWS and GCP
- Based on that assessment, security recommendations are provided
- Recommendations are implemented via Azure Policy

- Features
	- All recommen
	- Severity of recommendations (High, medium, low)
	- Resource health (Unhealthy, Healthy, Not applicable)
		- Unhealthy : A resource with an active recommendation
			- Healthy : A resource with no active recommendation
	- Status of the recommendation (completed or not)
	- Overdue recommendations
	- Recommendations drilling down for you resources using filters
	- Lightning bolt - Fix is available

---
### Secure Score

Entire purpose of Ss is to provide a number from 0 - 100, and this number tells you how secure you are for your various resources

![[Pasted image 20240131131940.png | 400]]

List of recomm that are associated with the MS secure score
- Features:
	- Recommendations which can increase the SS
		- Domains
			- Like enabling MFA
			- Secure Management Ports
			- Apply System Updates...
		- Have multiple recomm for a single domain
	- Max score and current score for a domain (progress bar)
	- Unhealthy resources categorized on domains

---
### Workbooks

Can also be used in Sentinel and Azure Monitor.

- Purposes - Can be used for
	- Data Visualization
	- Create Dashboards
- Workbooks allow for data visualization and dashboarding
- Workbooks can be built from scratch or customized
- Defender for Cloud includes prebuilt workbooks, such as:
	- Secure Score over Time
	- Vulnerability Assessment Findings
	- DevOps Security
	- System Updates
<br>
- Can be created from scratch or use pre-built workbooks
- Can use *KQL Query* to create dashboards and visualizations
- Also use *Add metric* option, for simplistic approach to create dashboards and visualizations using dropdowns.

![[Pasted image 20240131134232.png | 200]]

---
### Data Exporting

To export alerts and recommendations or vulnerability to other 3rd party tools.

You can stream the alerts and recommendations to the services so that the respective teams can get the expected alerts.

If you don't want to use Sentinel as a *Security information and event management (SIEM)* and want to use other SIEM solution, then you can leverage this feature of CSPM.

- ! We can Export the data to an EventHub or another log-analytic workspace
- There are built-in Azure tools for ensuring you can view your alert Data in all of the most popular solutions in use today, including:
	- Microsoft Sentinel
	- Splunk Enterprise and Splunk Cloud
	- IBM's QRadar
	- ServiceNow
	- ArcSight
	- Power Bl
	- Palo Alto Networks

- Features:
	- Export it to Event-Hub
	- Exported Date-Types
		- Which security *recomm* to export or all
		- *Recomm severity* (low, med, high)
		- *Secure score* export (all, Overall Score, Control Score)
			- Eg - only MFA Secure Score
		- Export *alerts* based on severity
		- Regulatory Compliance - ISO or Default - Azure - Sec - Benchmark
	- Frequency
		- Streaming Updates (real-time streaming)
		- Snapshots (specific snapshot).

---
### Remediations

Can be done manually and have another option which azure take cares to remediate the recomm, (when you click on Quick fix).

- Security recommendations should be remediated frequently
- Defender for Cloud always explains how to do this in general
- There is also a quick fix option available. Be careful if:
	- You manage resources with laC (you will introduce so-called *drift*)
	- You don't know the workload as you can heavily impact it by applying a fix Automatically

There will be a period called Freshness period which will tale 30 minutes to remove the recommendation after you click on *quick fix*

---
### Microsoft Cloud Security Benchmark

	Cloud Security > regulatory compliance

Renamed from
Azure Security Benchmark
⬇️
Microsoft Cloud Security Benchmark.

- Benchmark provides best practices and recommendations for your resources
- Control Domains
	- Network Security
	- Identity Management
	- Privileged Access
	- Data Protection
	- Asset Management
	- Logging and Threat Detection
	- Incident Response
	- Posture and Vulnerability Management
	- Endpoint Security
	- Backup and recovery
	- DevOps Security
	- Governance and Strategy

Lot of them are invented by Microsoft but. It also had a look at the most prominent industry benchmarks, like 

- CIS benchmarks
- NIST
- PCI
- DSS
- AWS well-Architected framework

<img src = "https://learn.microsoft.com/en-us/azure/defender-for-cloud/media/concept-regulatory-compliance/microsoft-security-benchmark.png"/>

Microsoft took and grouped under an umbrella, which is now called Microsoft Cloud Security Benchmark.

You can leverage the benchmark to identify recomm in defender for cloud and then also to address them accordingly.

- ! These domains have all the controls and these controls have sub-controls
![[Pasted image 20240131152745.png | 1000]]

---
## Defender CSPM (paid version)

### Governance Rules

	> Environment settings > Governance rules

- You can define rules that *assign an owner* and *add a due date* for Remediating a recommendation
- Governance rules drive accountability and you are providing an SLA as you are adding a due date for a recommendation to be remediated.

- You don't have to assign a stuff to an owner and add due dates individually for every recommendation.
- Using this feature, you will save your time and efficiency and secondly, reduce the errors because you define this automation once and then you can be sure that it is working correctly in the background all the time.

![[Pasted image 20240131155257.png | 1000]]

We can have multiple Governance rules with same conditions. So priority comes into play to choose which Governance rules to play around.

---
### Regulatory Compliance Standards

- Defender for Cloud assesses your resources against popular frameworks and regulatory standards, e.g.:
	- ISO 27001
	- NIST 800-53R4
	- PCl DSS v4
	- SOC 2 Type 2
	- CIS Microsoft Azure Foundations Benchmark

It accesses your compliance and security posture based on the controls of certain frameworks.

- Features
	- Manage compliance policies
	- Check compliance offerings
	- Audio reports of all Microsoft audits
	- Download reports of your compliance based on the selected compliance framework as PFD and CVE

---
### Cloud Security Explorer

- Cloud security Explorer is a very nice feature but not as capable as KQL.
- Cloud security explorer allows you to query resources and logs Without the need for writing KQL queries
- You can query all resources connected to Defender for Cloud.
- KQL still has more capabilities but the barrier to use cloud security Explorer is way lower
- You can also find Query templates to use.

---
### Attack Path Analysis

- Visualizes exploitable attack paths in your environment, e.g.:
	- VM containing critical vulnerabilities is exposed to the internet
	- VM containing critical vulnerabilities is exposed to the internet with read Permissions to a key vault
	- Azure Blob storage with sensitive data is publicly accessible

- This one actually visualizes certain exploitable attack paths.

---
### Agentless Scanning for VMs

- Requires Defender CSPM or Defender for Servers Plan 2.
- Available for Azure, AWS, and GCP.
- Zero Performance Impact.

![[Pasted image 20240131165319.png]]

- So the vulnerability scanning is done by an agent in a Virtual machine.
- Based on the CVE id, the captured vulnerability is sent to the defender for cloud.

- *agentless scanning* is a kind of scanning which is done on a snapshot of a disk, taken to a so-called *Isolated Scanning Environment* and scanned there.
- The scanning is not done on the actual work load which does not effect the performance of the VM.
- The Vulnerability engine scans the snapshot of the disk and results of the scan are fetched out and sent to the Defender for cloud.

---
### Agentless Discovery for Kubernetes

- API-based discovery of your Kubernetes clusters, their Configurations, and deployments
- Allows you to get an idea of your Kubernetes cluster configuration and its security posture.

---
### Container Registry Vulnerability Assessment

- Container Vulnerability Assessment is powered by MDVM (Microsoft Defender Vulnerability Management)
- Scans the Azure Container Registry (ACR) and identifies vulnerabilities
- Also included in Defender for Containers

---
### DevSecOps on Azure DevOps

![[Pasted image 20240131170944.png | 700]] 

---

### Agent based and agentless security protection

#### What is AGENT based protection?

cloud security is piece of agent that can be installed on cloud based or on-prem workloads.
its purpose is to gain or attain in-depth visibility.
It is deployed to configure real time threat protection and comprehensive monitoring of individual workloads.
and when Endpoint detection and response solutions(EDR) and security information and event management software (siem) are combined then we can get synthesized and corelated data and cross platform security incidents.

#### What is AGENTLESS based protection?

In this rather than deploying an agent within workloads to gather and transmit information , security data is collected using NON-INVASIVE methods , such as ==cloud image analysis== , ==log file analysis==  , and ==API  connections== .
This approach significantly reduces management overhead and negates the need for constant maintenance of the deployed agent.
Recently the industry is moving towards Agentless due to the advantages it offers , especially in large scale and complex cloud environments.
It gives seamless scalability, efficient resource consumption and reduced management complexity.


## scenario 
### AGENT based approach

It is beneficial if there is requirement of deep control and visibility over system process such as there  is mission critical system(like no electric power supply or internet connectivity or internet banking can be said as mission critical system).
So these system require a high level of  monitoring and management and control which only AGENT based can provide.

### AGENTLESS based approach 
Now the agentless protection basically provided by the Microsoft dfc is ideal for a situation where detailed monitoring is waste and impractical.
So for those scenarios the approach of AGENTLESS is preferred where they can protect by using invasive methods.


### Agentless capabilities
Defender for cloud CSPM is providing numerous features by agentless based approach.
It provides benefits such as ==attack path analysis , vulnerability scanning , data discovery== without the need of installing AGENTS.

| **Capability categories** | **Agent-based security** | **Agentless security** |
| ---- | ---- | ---- |
| Deployment and maintenance | Need deployment/ maintenance, but can be optimized to reduce overhead | No deployment requirement |
| Performance impact | Performance impact depends on type of agent deployed | No performance impact |
| Asset discovery and inventory | Can gain deeper insights into workloads for threat collection | API-based, fast and complete discovery |
| DevOps security | Not Applicable | DevOps Environment connectors helps community between DevOps security team |
| Posture management and compliance | Comprehensive visibility on details such as installed software and patches | - Contextual mapping of cloud resources<br>    <br>- Risk prioritization<br>    <br>- Analysis of lateral movement |
| Real-time threat response | Prevention, true real time threat detection& automatic response | Not main use case there because don’t have continuous monitoring |
| Data security | Cloud native security | - No deployment requirement<br>    <br>- No performance impact<br>    <br>- Risk prioritization |

### Benefits that make agent-based solutions an attractive choice:

- **Deep Threat Protection**: Agent-based solutions excel in their ability to provide deep threat protection. With Microsoft Defender for Servers, the agent installed on each server allows for thorough scanning and robust protection by actively monitoring system changes, detecting unusual behavior, and responding instantly to potential security threats.
- **Frequent Scanning**: Agent-based solutions enable more frequent scanning of servers. Since the agent is directly installed on the server, it can conduct real-time, continuous scanning, ensuring threats are detected and addressed swiftly.
- **Leveraging File Integrity Monitoring (FIM****)**: File Integrity Monitoring is a vital feature of Microsoft Defender for Servers. FIM enables you to track and record any changes made to critical system files, configuration files, and content files. It helps identify unexpected or unauthorized changes that may signal a security breach.
- **Utilizing Adaptive Application Controls**: A distinguishing feature of Microsoft Defender for Servers, Adaptive Application Controls, uses machine learning to analyze running applications on your servers and create a list of safe software. When these controls are configured, they alert you when any application runs outside those identified as safe. This enhances your server security by aiding in the identification of potential malware, maintaining compliance with local security policies, recognizing outdated or unsupported applications, and increasing oversight of apps accessing sensitive data.
- **Deep System Insights**: An agent-based solution provides detailed insights into the server’s system processes. The agent's access to and monitoring of server activities on a granular level aid in understanding intricate patterns and anomalies that could indicate potential threats.
- **Actionable Remediation**: Agent-based solutions often offer immediate and effective remediation actions during a security event. Because the agent is housed within the server, it can take rapid corrective measures, minimizing the potential impact of a security breach.

### benefits of agentless in dcspm:

- Simplified deployment and scalability
- efficient resource consumption
- Cross platform compatibility and flexibility
- reduces management complexity and workload on managing the security


#### Services by Microsoft defender for cloud

- Attack path analysis
- vulnerability scanning
- data discovery
- container and image scanning

---

#### basic difference between CSPM and CWP

CSPM tools analyze the cloud setups to search for vulnerabilities, misconfigurations, and compliance violations. They offer suggestions for how the situation might be improved.
whereas 



----

### What is infrastructure service insights ?

|   |   |   |   |
|---|---|---|---|
|[Infrastructure service insights](https://learn.microsoft.com/en-us/azure/defender-for-cloud/asset-inventory)|Diagnose weaknesses in your application infrastructure that can leave your environment susceptible to attack.|- [Identify attacks targeting applications running over App Service](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-app-service-introduction)  <br>- [Detect attempts to exploit Key Vault accounts](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-key-vault-introduction)  <br>- [Get alerted on suspicious Resource Manager operations](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-resource-manager-introduction)  <br>- [Expose anomalous DNS activities](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-dns-introduction)|- Defender for App Service  <br>- Defender for Key Vault  <br>- Defender for Resource Manager  <br>- Defender for DNS|
Infrastructure service insights means that any application or the app service has some backend working for its security and consistency is referred to as a infrastructure service and for having a lookout or monitoring for that backend can be referred as its insights.

so basically in cloud workload protection there are plans like 
- ==DEFENDER FOR APP SERVICE , 
- ==defender for DNS and 
- ==defender for resource manager and also 
- ==defender for key vaults== 

so they all work on the backend and have insights of the infrastructure.
so DNS check that there is no data exfiltration  , 
app service will check there is no anomalies 
key vault will safeguards encryption keys and secrets like certificates, connection strings.

If we talk about use case scenarios then it can be said that
Agent-based security is highly suitable for networks with standard configurations andback case and guard systems and also has simple workloads. Importantly, agent-based security is highly secured and effective in collecting data, but you have to configure it for each endpoint manually.

Agentless security will be an ideal choice for your organization if you are looking for a solution that will be compatible with your large and complex network. This solution is more suited to work with workloads that are constantly changing and for environments that work with APIs.

Agent-based security also can enforce security policies, perform scans, and mitigate vulnerability when the network is offline, but agentless is entirely dependent on the network.

-------


**Agent-based Scanning:**

**More likely to benefit:**

- **Organizations with high-security needs:** Banks, healthcare providers, and government agencies dealing with sensitive data often require deep in-depth vulnerability assessments and real-time threat detection. Agent-based solutions offer granular control and comprehensive protection.
- **Organizations with manageable IT infrastructure:** Companies with a limited number of critical systems on-premises benefit from the focused protection of agents. Installation and management become less of a burden in smaller, controlled environments.
- **Organizations with sufficient resources:** Running agents requires additional processing power and memory. Companies with robust IT infrastructure and budget allocated for security can handle the resource impact.

**Agentless Scanning:**

**More likely to benefit:**

- **Large, distributed organizations:** Companies with thousands of devices across various locations, including BYOD environments, find agentless scanning easier to deploy and manage. The lightweight nature is suitable for scaling across diverse endpoints.
- **Organizations with dynamic environments:** Cloud-based companies or those with frequent infrastructure changes benefit from the agentless approach's ability to scan without requiring installation on every new device.
- **Organizations with resource constraints:** Companies with limited processing power or budget constraints find agentless scanning less resource-intensive, making it a more feasible option.

**Hybrid Approach:**

**Most organizations benefit from a hybrid approach:** This combines the strengths of both methods for comprehensive coverage. Critical systems can have agents for in-depth protection, while other endpoints leverage agentless scanning for broader reach and easier management.


----

### Defender for key vaults

Azure Key Vault is a cloud service that safeguards encryption keys and secrets like certificates, connection strings, and passwords.

Enable **Microsoft Defender for Key Vault** for Azure-native, advanced threat protection for Azure Key Vault, providing an additional layer of security intelligence.

#### benefits of Microsoft Defender for Key Vault

- detects unusual and potentially harmful attempts to access or exploit Key Vault accounts
- shows alerts and optionally sends them via email to relevant members of your organization.
- alerts include the details of the suspicious activity and recommendations on how to investigate and remediate threats.
- Alerts from Microsoft Defender for Key Vault includes these elements:
	- Object ID, User Principal Name or IP address of the suspicious resource


#### Investigation & Remediation Steps

Step 1 :
	Identify the source - if it is insider or known then investigate and ask if it is legitimate or not.
Step 2: if not identified then check the IP associated with the alert which tried to access. enable the azure key vault firewall
	may be unknown application tried to access. check the security principals or the best practices and restrict the operations.
step 3: if the alert shows the microsoft entra role in ur tenant then reach out to the administrator and determine how to reduce or revoke the permissions.

basic Alerts  types - for azure key vaults 
- **Access from a suspicious IP address to a key vault**
- **Access from a TOR exit node to a key vault**
- **High volume of operations in a key vault**
- **Unusual application accessed a key vault**

---

### Defender for resource manager

- It is the deployment and management service for Azure
- It provides a management layer that enables you to create, update, and delete resources in your Azure account
- we can use management features like locks and tags for organizing the resources
- Since it manages all the azure resources therefore it is the potential target for the adversaries to attack.
- It reviews all the operations performed - whether they are performed from portal or azure CLI 
- Defender for resource manager will run advanced security analytics to detect threats and alerts you about suspicious activity.


#### benefits of Microsoft Defender for Resource Manager
- **Suspicious resource management operations**, such as operations from malicious IP addresses, disabling antimalware, and suspicious scripts running in VM extensions
- **Use of exploitation toolkits** like Microburst or PowerZure
- **Lateral movement** from the Azure management layer to the Azure resources data plane


#### Investigation & Remediation Steps

- verify with the resource owner if the activity was expected or not , if not then treat the related user accounts, subscriptions, and virtual machines as compromised and mitigate the issue.



----
•Microsoft Defender vulnerability management

•Automatic agent onboarding, alert and data integration

•Generates detailed, context-based, security alerts easily integrated with any SIEM

•Attack Surface Reduction

•Automatic Investigation and Response


---
