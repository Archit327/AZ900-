how to perform GRC 
and how to work in azure 
CCO dashboard
What is GRC?
Governance , Risk, Compliance

| Time Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 1: GRC introduction , Its Components and how it's utilized in an organization<br><br>Day 2: Governance , Azure Governance<br><br>Day 3: Blueprints, CCO dashboard<br><br>Day 4: Query , Presentation (Real customer requirement approach)<br><br>Day 5: Risk Assessment Introduction, Azure RA( focused on Gov), Process and documentation<br><br>Day 6: Query , Presentation<br><br>Day 7: Compliance introduction, Requirements<br><br>Day 8 : Regulations , laws and Act ;<br><br>Day 9: How to review a standard<br><br>Day 10: Query, Presentation |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| What is governance risk compliance?<br>Why do org need it?<br>Governance in Azure - How does it work, what are its components?<br><br>                                                                                                                                                                                                                                                                                                                                                                                                                      |

## Day 1
### What is GRC
GRC stands for Governance - Risk - Compliance.
#### Governance

**Governance in cloud basically means governing that the RIGHT PEOPLE ARE DEPLOYING RIGHT RESOURCES WITH RIGHT CONFIGURATIONS. **

It  is a set of policies, rules, or frameworks that a company uses to achieve its business goals. It defines the responsibilities of administrator, such as the board of directors and senior management.
A good org would have governance as in
- **Ethics** - like proper punch in punch out
- **Accountability** - the higher authority should be answerable to any mishappening
- **Information sharing** - there should be transparency in some aspect
- **Policies** - set of rules which each and every employee should take care of

#### Risk
There can be any type of risk
- Financial risk
- physical risk
- Legal Risks
- Data breach risk
A good org will have proper risk management so that they can find security loopholes.
They will have time to time risk assessment procedures to have uptodate and have full command on the Environment.

#### Compliance
Compliance is the act of following rules, laws, and regulations. It applies to legal and regulatory requirements set by industrial bodies and also for internal corporate policies.
In GRC, compliance involves implementing procedures to ensure that business activities comply with the respective regulations. For example, healthcare organizations must comply with laws like HIPAA that protect patients' privacy.



![[Pasted image 20240829153411.png]]

## Governance in Azure

### Scope 
The level of hierarchy which will form azure environment.
![[Pasted image 20240829162347.png | 600]]

Their can be 6 level of Management grp as well

### RBAC
Role based Access Control
This maintains who can access what and what extent.
We have certain role for subscriptions basis.
- OWNER
- Contributor
- Reader
### Benefits of RBAC roles
- just restricts who can perform what 
- the role provided at high level is inherited to children scope
- roles can be applied at any scope
- custom role can be created 

### Resource Locks
 resource locks can be used for blocking at more granular level. 
 This can be applied to any resource and at any scope like subscription or resource grp. 
 and it will also be inherited from parent scope to child scope.
 There are two types of LOCK TYPE-
 - Read Only - the user can only read the resource but cannot make any changes and delete it.
 - delete - the user cannot delete the resource.


## Policy
Real time enforcement, compliance assessment and remediation at scale.
- we can do real time policy enforcements
- policy will help monitoring and alerting.
- it can be assigned to management grp , subcription and resource grps and those will be inherited to child scope.



---



**Azure Governance** refers to a set of tools, services, and best practices in Microsoft Azure that help organizations manage and control their cloud resources effectively. It ensures that all resources in Azure (like virtual machines, databases, and storage) are used in a way that aligns with the organization's policies, compliance requirements, and overall goals.

### **In Simple Terms:**
Azure Governance is about **keeping your cloud environment organized, secure, and compliant**. It helps you make sure that:
- **Resources are properly organized** (e.g., by department or project).
- **Costs are controlled** and not exceeded.
- **Security policies** are enforced (e.g., who can access what).
- **Compliance requirements** are met (e.g., data handling rules).

### **Key Components of Azure Governance:**

1. **Azure Policy**:
   - Enforces rules and guidelines for resources. For example, you can create policies to ensure that only certain regions are used for deploying resources or to require specific security settings.

2. **Azure Blueprints**:
   - Provides a way to define and deploy a repeatable set of Azure resources, following a consistent set of rules and standards. It helps in setting up environments quickly while ensuring they are compliant.

3. **Role-Based Access Control (RBAC)**:
   - Manages who has access to Azure resources and what they can do with them. It ensures that users only have the permissions they need to do their jobs, enhancing security.

4. **Resource Tags**:
   - Helps organize and manage resources by applying labels (tags) to them. This makes it easier to categorize and search for resources, track usage, and manage costs.

5. **Management Groups**:
   - Allows you to organize and manage multiple Azure subscriptions under a single structure, applying policies and controls at a higher level, which are then inherited by all underlying subscriptions.

6. **Cost Management and Billing**:
   - Provides tools to monitor and control cloud spending. This helps organizations stay within budget and optimize their cloud costs.

### **Why is Azure Governance Important?**

- **Maintains Control**: Helps maintain control over a growing number of cloud resources and ensures they are managed according to organizational policies.
- **Improves Security**: Ensures resources are secure and access is controlled.
- **Ensures Compliance**: Helps meet regulatory requirements and standards by enforcing policies and providing audit trails.
- **Reduces Costs**: Helps monitor and control spending, avoiding unnecessary costs.

### **Summary:**
Azure Governance is like setting rules and guidelines for using cloud resources, so everything is organized, secure, compliant, and cost-effective. It helps organizations use Azure effectively while maintaining control and ensuring resources are used responsibly.


---

The **Cisco Cloud Consumption Optimization (CCO) Dashboard** is a tool that provides insights and visibility into an organization's cloud usage. It helps IT and cloud teams monitor, manage, and optimize their cloud resources to reduce costs, enhance performance, and ensure compliance with organizational policies.

### **Key Components of the CCO Dashboard:**

1. **Dashboard Overview:**
   - **Summary View**: Provides an overview of the total cloud resources in use, including the number of services, total spend, and trends over time.
   - **Cloud Accounts**: Lists the various cloud accounts being monitored, such as AWS, Azure, and Google Cloud, with details on usage and costs associated with each.

2. **Cost Management:**
   - **Cost Breakdown**: Displays how costs are distributed across different cloud providers, services, and regions. It also provides detailed insights into which resources are driving costs.
   - **Cost Trends**: Shows historical data and trends in cloud spending, helping to identify patterns and opportunities for cost savings.

3. **Resource Management:**
   - **Resource Inventory**: Lists all cloud resources across different providers, including virtual machines, storage accounts, databases, and more. It helps in tracking resource utilization.
   - **Utilization Metrics**: Provides data on the usage and performance of cloud resources, helping identify underutilized or idle resources that can be optimized or decommissioned.

4. **Optimization Recommendations:**
   - **Cost Optimization**: Offers actionable recommendations to reduce cloud spend, such as rightsizing instances, shutting down unused resources, or moving to more cost-effective pricing models.
   - **Performance Optimization**: Provides suggestions to improve resource performance, such as adjusting instance types, improving storage configurations, or modifying network setups.

5. **Compliance and Governance:**
   - **Policy Enforcement**: Ensures that cloud resources comply with organizational policies, such as tagging standards, region restrictions, or security configurations.
   - **Security Insights**: Provides information on potential security risks, such as open ports, misconfigured access controls, or outdated software versions.

6. **Reporting and Alerts:**
   - **Custom Reports**: Allows users to generate detailed reports on cloud usage, costs, and compliance for different departments or projects within the organization.
   - **Alerts and Notifications**: Sends alerts when certain thresholds are met, such as exceeding budget limits, underutilization of resources, or policy violations.

### **Benefits of Using the CCO Dashboard:**

- **Cost Savings**: By providing insights into cloud usage and offering optimization recommendations, the CCO Dashboard helps organizations reduce unnecessary cloud spending.
- **Improved Visibility**: Offers a centralized view of cloud resources across multiple providers, making it easier to manage and monitor the cloud environment.
- **Enhanced Compliance**: Ensures that all cloud resources comply with organizational policies and industry regulations, reducing the risk of non-compliance.
- **Performance Optimization**: Helps improve the performance of cloud resources by identifying and addressing inefficiencies in the cloud environment.

### **Getting Started with the CCO Dashboard:**

1. **Access the Dashboard**: Log in to the Cisco portal where the CCO Dashboard is hosted.
2. **Connect Cloud Accounts**: Integrate your cloud provider accounts (e.g., AWS, Azure, Google Cloud) with the dashboard to start collecting data.
3. **Explore the Interface**: Familiarize yourself with the various sections of the dashboard, such as cost management, resource inventory, and optimization recommendations.
4. **Review and Act**: Regularly review the dashboard for insights and recommendations, and take action to optimize cloud usage and costs.

### **Conclusion:**

The Cisco Cloud Consumption Optimization (CCO) Dashboard is a powerful tool for organizations looking to optimize their cloud environment. It provides deep insights into cloud usage, costs, and compliance, enabling better decision-making and more efficient cloud management.


----

  

Select

Severity

Informational

Detect time

08:30:02

Detection name

Unusual login to an endpoint

Account name

alexas

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-kgiorda-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:12:02

Detection name

Unusual login to an endpoint

Account name

johnm

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-rroetm2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:10:02

Detection name

Unusual login to an endpoint

Account name

drashtip

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-mlaw4-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:04:02

Detection name

Unusual login to an endpoint

Account name

paulp

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-dbohrer-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:01:02

Detection name

Unusual login to an endpoint

Account name

erikk

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-jlawren-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Sep. 3, 2024

---

Select

Severity

Low

Detect time

20:30:05

Detection name

Suspicious web-based activity (ML)

Account name

akkumar

Source endpoint IP address

24.102.145.15

Country

United States

Destination application identifier

Microsoft Intune Web Company Portal

Source endpoint IP reputation

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:32:02

Detection name

Unusual login to an endpoint

Account name

alexandrah

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-lstrom3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:27:02

Detection name

Unusual login to an endpoint

Account name

rodneya

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-bpitony-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:21:02

Detection name

Unusual login to an endpoint

Account name

rajanir

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-achilin-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:11:02

Detection name

Unusual login to an endpoint

Account name

ryanfe

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-lpiriz-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:04:02

Detection name

Unusual login to an endpoint

Account name

ankeshm

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-lgelfan-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:01:02

Detection name

Unusual login to an endpoint

Account name

Jhanvika

Account domain

ADS.MHAINC.COM

Source endpoint name

van-khasta2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:56:03

Detection name

Unusual login to an endpoint

Account name

tylerg

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jkatz-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:29:02

Detection name

Unusual login to an endpoint

Account name

BrianRa

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-aaquil1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:12:03

Detection name

Unusual access to a user entity

Account name

dhanapals

Account domain

ADS.MHAINC.COM

Source endpoint name

PHY-DSELVA-W10

Destination SPN

mssqlsvc/flo-sql-tdmp.ads.mhainc.com:1433

Destination endpoint

SRVFLO-SQL-TALEND

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:28:02

Detection name

Unusual login to an endpoint

Account name

burton

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-tcarrol-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:04:02

Detection name

Unusual login to an endpoint

Account name

alyssaw

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-tdoughe-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:03:02

Detection name

Unusual login to an endpoint

Account name

NiharikaM

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-rvillaf-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:00:02

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-ppistor-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:54:02

Detection name

Unusual login to an endpoint

Account name

anthonyd

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-rnilsso-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:51:02

Detection name

Unusual login to an endpoint

Account name

farham

Account domain

ADS.MHAINC.COM

Source endpoint name

talendbrowse.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:33:02

Detection name

Unusual login to an endpoint

Account name

jteston.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-msaleem-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:40:02

Detection name

Unusual login to an endpoint

Account name

wandal

Account domain

ADS.MHAINC.COM

Source endpoint name

talendbrowse.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:32:05

Detection name

Access from IP with bad reputation

Account name

tvalentino.admin@ads.mhainc.com

Source endpoint IP address

52.136.113.82

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

12:17:02

Detection name

Unusual login to an endpoint

Account name

MichelleT

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-smaze2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:07:05

Detection name

Access from IP with bad reputation

Account name

DSelvaraj@mhainc.com

Source endpoint IP address

13.90.136.68

Country

United States

Destination application identifier

Microsoft Power BI

Source endpoint IP reputation

Hosting facility

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

12:07:05

Detection name

Access from IP with bad reputation

Account name

SRVFLO-POWERBI@ads.mhainc.com

Source endpoint IP address

13.90.136.68

Country

United States

Destination application identifier

Microsoft Power BI

Source endpoint IP reputation

Hosting facility

Assigned to

Abhilash Reddy

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

12:02:05

Detection name

Access from IP with bad reputation

Account name

jcharlton.admin2@ads.mhainc.com

Source endpoint IP address

4.236.134.146

Country

United States

Destination application identifier

Microsoft Office 365 Portal

Source endpoint IP reputation

Hosting facility

Assigned to

Abhilash Reddy

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

11:44:02

Detection name

Unusual login to an endpoint

Account name

douglasb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pdire-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:31:05

Detection name

Access from IP with bad reputation

Account name

smaze.admin@ads.mhainc.com

Source endpoint IP address

20.242.207.126

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:26:05

Detection name

Access from IP with bad reputation

Account name

fkao.admin@mhainc.com

Source endpoint IP address

52.224.220.82

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:18:02

Detection name

Unusual login to an endpoint

Account name

marges

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-nkaprol-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:55:01

Detection name

Access from IP with bad reputation

Account name

SRVFLO-NETWRIX@ads.mhainc.com

Source endpoint IP address

52.226.123.31

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

10:30:02

Detection name

Unusual login to an endpoint

Account name

patd

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-cseast-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:28:02

Detection name

Unusual login to an endpoint

Account name

anthonyd

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-grossna-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:07:24

Detection name

Use of stale user account

Account name

SRVFLO-TALCL-U

Account domain

ADS.MHAINC.COM

Days inactive

1532

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

09:58:03

Detection name

Unusual login to an endpoint

Account name

bshah.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-app-coga-p1.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:58:02

Detection name

Unusual login to an endpoint

Account name

paulb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-mbordoy-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:32:02

Detection name

Unusual login to an endpoint

Account name

Harshada

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-qdavis-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:31:02

Detection name

Unusual login to an endpoint

Account name

douglasb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-iwilso2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:11:03

Detection name

Unusual login to an endpoint

Account name

briannap

Account domain

ADS.MHAINC.COM

Source endpoint name

van-lboure1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:01:02

Detection name

Unusual login to an endpoint

Account name

robertv

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-mcollin-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:37:02

Detection name

Unusual login to an endpoint

Account name

Rob

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-vmakan3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Low

Detect time

06:45:05

Detection name

Suspicious web-based activity (ML)

Account name

matthew.berra

Source endpoint IP address

63.78.151.2

Country

United States

Destination application identifier

Zscaler Private Access (ZPA)

Source endpoint IP reputation

--

Assigned to

Datha Chandu

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

03:28:02

Detection name

Unusual login to an endpoint

Account name

jeffh

Account domain

ADS.MHAINC.COM

Source endpoint name

van-sausti2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Sep. 2, 2024

---

Select

Severity

Informational

Detect time

23:50:02

Detection name

Unusual login to an endpoint

Account name

donnae

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-spaik2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

22:22:02

Detection name

Unusual login to an endpoint

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-smaze2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

21:05:02

Detection name

Unusual login to an endpoint

Account name

annen

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-ktully-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:43:02

Detection name

Unusual login to an endpoint

Account name

tomh

Account domain

ADS.MHAINC.COM

Source endpoint name

van-swakef3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:23:02

Detection name

Unusual login to an endpoint

Account name

Kristina

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-msaleem-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:28:02

Detection name

Unusual login to an endpoint

Account name

AnthonyT

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-smaze2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:15:02

Detection name

Unusual login to an endpoint

Account name

luiss

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-tdoughe-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:31:02

Detection name

Unusual login to an endpoint

Account name

MaryellM

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-jmceach-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

06:10:04

Detection name

Access from IP with bad reputation

Account name

CW.Admin@ads.mhainc.com

Source endpoint IP address

172.172.181.79

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

05:19:04

Detection name

Access from IP with bad reputation

Account name

AKumar@mhainc.com

Source endpoint IP address

40.117.124.189

Country

United States

Destination application identifier

Microsoft Power BI

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Low

Detect time

04:49:05

Detection name

Suspicious web-based activity (ML)

Account name

akumar

Source endpoint IP address

103.169.84.34

Country

India

Destination application identifier

--

Source endpoint IP reputation

--

Assigned to

Unassigned

Resolution

--

Status

New

Sep. 1, 2024

---

Select

Severity

Informational

Detect time

23:49:02

Detection name

Unusual login to an endpoint

Account name

dianac

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-dfranke-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:40:02

Detection name

Unusual login to an endpoint

Account name

marcb

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-jspurg2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

06:10:02

Detection name

Unusual login to an endpoint

Account name

stephaniem

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jrizal-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Aug. 31, 2024

---

Select

Severity

Informational

Detect time

20:55:02

Detection name

Unusual login to an endpoint

Account name

allisonb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-kbejar3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:24:02

Detection name

Unusual login to an endpoint

Account name

mayurs

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-smaze2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:24:02

Detection name

Unusual login to an endpoint

Account name

myamma.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

vm-myamma-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:35:02

Detection name

Unusual login to an endpoint

Account name

stefaniea

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ckapfer-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:18:02

Detection name

Unusual login to an endpoint

Account name

ThomasF

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-dstauf-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:15:02

Detection name

Unusual login to an endpoint

Account name

nashwai

Account domain

ADS.MHAINC.COM

Source endpoint name

van-mfries-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:02:02

Detection name

Unusual login to an endpoint

Account name

quinnw

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-smaze2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:18:02

Detection name

Unusual login to an endpoint

Account name

mandim

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-skoen-cn5ry.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:05:02

Detection name

Unusual login to an endpoint

Account name

robertv

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-omustaf-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

03:19:02

Detection name

Unusual login to an endpoint

Account name

ThomasF

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-jblair-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

03:00:02

Detection name

Unusual login to an endpoint

Account name

Kristina

Account domain

ADS.MHAINC.COM

Source endpoint name

van-mfries-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Aug. 30, 2024

---

Select

Severity

Informational

Detect time

23:07:05

Detection name

Suspicious web-based activity (ML)

Account name

spensoy.admin

Source endpoint IP address

209.118.66.70

Country

United States

Destination application identifier

Microsoft Office 365 Portal

Source endpoint IP reputation

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

22:27:02

Detection name

Unusual login to an endpoint

Account name

michaell

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-wobrie3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:19:03

Detection name

Unusual login to an endpoint

Account name

jamiee

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-tdeboe-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

18:08:02

Detection name

Unusual login to an endpoint

Account name

ThomasF

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-rfenner-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:30:02

Detection name

Unusual login to an endpoint

Account name

muhammado

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jhurban-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:49:02

Detection name

Unusual login to an endpoint

Account name

michaelmc

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-ljochm4-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:28:02

Detection name

Unusual login to an endpoint

Account name

twinklep

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-npanda2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:21:04

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-spaik2-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:21:02

Detection name

Unusual login to an endpoint

Account name

timmib

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-nmunipa-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:00:02

Detection name

Unusual login to an endpoint

Account name

jasonw

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-skoen-cn5ry.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:48:02

Detection name

Unusual login to an endpoint

Account name

catherinek

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-tvalen3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:59:05

Detection name

Access from IP with bad reputation

Account name

bshah.admin@ads.mhainc.com

Source endpoint IP address

20.228.223.18

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:31:02

Detection name

Unusual login to an endpoint

Account name

abioduna

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-efleck2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:19:05

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-spaik2-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:29:02

Detection name

Unusual login to an endpoint

Account name

luiss

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-jmanzi1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:59:02

Detection name

Unusual login to an endpoint

Account name

joanr

Account domain

ADS.MHAINC.COM

Source endpoint name

ric-sdoshi3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:39:02

Detection name

Unusual login to an endpoint

Account name

elenaa

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ckapfer-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:37:02

Detection name

Unusual login to an endpoint

Account name

cindys

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ccarver-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:21:02

Detection name

Unusual login to an endpoint

Account name

tammyk

Account domain

ADS.MHAINC.COM

Source endpoint name

van-lboure1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:17:02

Detection name

Unusual login to an endpoint

Account name

thomasd

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ekarali-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:58:02

Detection name

Unusual login to an endpoint

Account name

timothym

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-rvillaf-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:33:04

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dcecer2-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:21:02

Detection name

Unusual login to an endpoint

Account name

muhammads

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dcoste2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:20:02

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-msims-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:17:02

Detection name

Unusual login to an endpoint

Account name

nitak

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-adelvis-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:11:05

Detection name

Unusual login to an endpoint

Account name

allyc

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-atozzi2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:11:04

Detection name

Unusual login to an endpoint

Account name

dianac

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-badensa-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:02:02

Detection name

Unusual login to an endpoint

Account name

rodneya

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-jharris-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:51:02

Detection name

Unusual login to an endpoint

Account name

rachaela

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jhayman-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:41:02

Detection name

Unusual login to an endpoint

Account name

jamilap

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-abaker-w11.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:58:02

Detection name

Unusual login to an endpoint

Account name

dawnt

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ccarver-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:49:02

Detection name

Unusual login to an endpoint

Account name

michaeldi

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-alavign-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:34:02

Detection name

Unusual login to an endpoint

Account name

dianac

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-bhewit1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:04:02

Detection name

Unusual login to an endpoint

Account name

samuelm

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-mwahnon-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

05:36:02

Detection name

Unusual access to a user entity

Account name

bhagyasreen

Account domain

ADS.MHAINC.COM

Source endpoint name

AZE-INF-ACCD1

Destination SPN

mssqlsvc/aze-sql-fin-t1.ads.mhainc.com:1433

Destination endpoint

SRVGBL-SQLSVR-01

Assigned to

Datha Chandu

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

05:28:04

Detection name

Access from IP with bad reputation

Account name

SHSingh@mhainc.com

Source endpoint IP address

20.185.198.250

Country

United States

Destination application identifier

Microsoft Power BI

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

04:25:02

Detection name

Unusual login to an endpoint

Account name

byrong

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-atozzi2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

01:34:02

Detection name

Unusual login to an endpoint

Account name

SVRFLO-FS-Talend

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-kjagun-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

00:36:02

Detection name

Unusual login to an endpoint

Account name

SRVFLO-POWERBI

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-dselva-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Aug. 29, 2024

---

Select

Severity

Informational

Detect time

23:00:02

Detection name

Unusual login to an endpoint

Account name

lindsayg

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-sthodim-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

21:12:02

Detection name

Unusual login to an endpoint

Account name

skoenitzer.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-sql-etl.ads.mhainc.com

Assigned to

Unassigned

Resolution

False positive

Status

New

Select

Severity

Informational

Detect time

20:11:02

Detection name

Unusual login to an endpoint

Account name

kimberly

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-therme4-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

20:05:02

Detection name

Unusual login to an endpoint

Account name

David

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-bdastr1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:13:02

Detection name

Unusual login to an endpoint

Account name

Kristina

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dcecer2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:12:02

Detection name

Unusual login to an endpoint

Account name

lennorem

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-kfogar2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:11:02

Detection name

Unusual login to an endpoint

Account name

garyr

Account domain

ADS.MHAINC.COM

Source endpoint name

van-jcharl1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

19:00:02

Detection name

Unusual login to an endpoint

Account name

joanr

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-rvillaf-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:40:02

Detection name

Unusual login to an endpoint

Account name

mariay

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-smoll2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

17:00:02

Detection name

Unusual login to an endpoint

Account name

quianaf

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-fmohiud-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:39:02

Detection name

Unusual login to an endpoint

Account name

paulb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-rradhak-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:36:04

Detection name

Access from IP with bad reputation

Account name

MNizamuddin@mhainc.com

Source endpoint IP address

20.185.198.250

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Abhilash Reddy

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

16:03:02

Detection name

Unusual login to an endpoint

Account name

SRVFLO-SQL-REPDD

Account domain

ADS.MHAINC.COM

Source endpoint name

netrxexports.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:53:02

Detection name

Unusual login to an endpoint

Account name

jeffwi

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-bdastr1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:52:02

Detection name

Unusual login to an endpoint

Account name

michaelmc

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-mombali-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:29:05

Detection name

Access from IP with bad reputation

Account name

SRVFLO-FinanceApps@ads.mhainc.com

Source endpoint IP address

20.231.17.15

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:45:02

Detection name

Unusual login to an endpoint

Account name

ryanfe

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-tdoughe-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:38:02

Detection name

Unusual login to an endpoint

Account name

ThomasF

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-srutzle-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:35:02

Detection name

Unusual login to an endpoint

Account name

laurenl

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-jgrahn2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:29:02

Detection name

Unusual login to an endpoint

Account name

joshuah

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-kcordel-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:18:05

Detection name

Access from IP with bad reputation

Account name

spensoy.admin@ads.mhainc.com

Source endpoint IP address

52.136.113.82

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:59:02

Detection name

Suspicious LDAP search (AD-CS reconnaissance)

Account name

pkanakamedala

Account domain

ADS.MHAINC.COM

Source endpoint name

--

Domain controller name

aze-inf-dc03.ads.mhainc.com

LDAP signature

Active Directory Certificate Services Enumeration

Assigned to

Abhilash Reddy

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

13:52:02

Detection name

Unusual login to an endpoint

Account name

michaelmc

Account domain

ADS.MHAINC.COM

Source endpoint name

van-wmorro2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:45:02

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-rortiz-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:31:02

Detection name

Unusual login to an endpoint

Account name

kristenf

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-porndof-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:13:09

Detection name

Identity verification timed out

Account name

radams.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

EXT-JSPURG2-W10.ads.mhainc.com

Policy rule name

FC - RDP Access to DCs

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:59:10

Detection name

Unusual login to an endpoint

Account name

glenm

Account domain

ADS.MHAINC.COM

Source endpoint name

van-lboure1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:52:04

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-spaik2-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:52:02

Detection name

Unusual login to an endpoint

Account name

rodneya

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-tgraf2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:50:05

Detection name

Access from IP with bad reputation

Account name

recrxdocs@net-rx.com

Source endpoint IP address

172.190.170.199

Country

United States

Destination application identifier

Office 365 Exchange Online

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:43:10

Detection name

Unusual login to an endpoint

Account name

rachaela

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-kcordel-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:17:10

Detection name

Unusual login to an endpoint

Account name

SheelaM

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-bhewit1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:03:10

Detection name

Unusual login to an endpoint

Account name

JessicaH

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-tparik-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:02:02

Detection name

Unusual login to an endpoint

Account name

calm

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-trechne-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:42:52

Detection name

Privilege escalation (user)

Account name

abreddy

Account domain

ADS.MHAINC.COM

Added privileges

Azure security privileges

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

11:41:25

Detection name

Privilege escalation (user)

Account name

DChandu

Account domain

ADS.MHAINC.COM

Added privileges

Azure security privileges

Assigned to

Archit Jain

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

11:13:10

Detection name

Unusual login to an endpoint

Account name

virginias

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-tcarrol-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:53:02

Detection name

Unusual login to an endpoint

Account name

byrong

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pbutler-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:47:10

Detection name

Unusual login to an endpoint

Account name

gennadys

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-fhooey-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:44:10

Detection name

Unusual login to an endpoint

Account name

claudiak

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-cseast-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:25:03

Detection name

Unusual login to an endpoint

Account name

neelanjanap

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-anapier-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:08:05

Detection name

Access from IP with bad reputation

Account name

jspurgeon.admin@mhainc.com

Source endpoint IP address

40.80.144.70

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Datha Chandu

Resolution

False positive

Status

Closed

Select

Severity

Informational

Detect time

10:05:10

Detection name

Unusual login to an endpoint

Account name

michaelmc

Account domain

ADS.MHAINC.COM

Source endpoint name

van-kskoka2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:59:10

Detection name

Unusual login to an endpoint

Account name

NiharikaM

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-eoveren-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:45:03

Detection name

Unusual login to an endpoint

Account name

rajanir

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-adelvis-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:32:02

Detection name

Unusual login to an endpoint

Account name

douglasb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-mmcclu2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:25:02

Detection name

Unusual login to an endpoint

Account name

joanr

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-abroad3-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:10:10

Detection name

Unusual login to an endpoint

Account name

Vivek

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-jreed2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:50:48

Detection name

Identity verification timed out

Account name

radams.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

FLO-VSINGH-W10.ads.mhainc.com

Policy rule name

FC - RDP Access to DCs

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:38:10

Detection name

Unusual login to an endpoint

Account name

kyler

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-abaker-w11.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:36:10

Detection name

Unusual login to an endpoint

Account name

meganp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dcoste2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:33:10

Detection name

Unusual login to an endpoint

Account name

michaelmc

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jpascal-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

08:24:10

Detection name

Unusual login to an endpoint

Account name

briannab

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-lmurph2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

07:51:10

Detection name

Unusual login to an endpoint

Account name

stephenm

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-mschube-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

03:41:10

Detection name

Unusual login to an endpoint

Account name

ryanb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pbutler-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

02:15:09

Detection name

Identity verification timed out

Account name

BDas

Account domain

ADS.MHAINC.COM

Source endpoint name

--

Policy rule name

FC - Inactive User Access

Assigned to

Unassigned

Resolution

--

Status

Closed

Select

Severity

Informational

Detect time

01:24:10

Detection name

Unusual login to an endpoint

Account name

gregorys

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-radam-6fti9.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

00:43:05

Detection name

Access from IP with bad reputation

Account name

portal@mhainc.com

Source endpoint IP address

52.191.229.166

Country

United States

Destination application identifier

Office 365 Exchange Online

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Aug. 28, 2024

---

Select

Severity

Informational

Detect time

20:58:10

Detection name

Unusual login to an endpoint

Account name

skoenitzer.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-app-acct-p1.ads.mhainc.com

Assigned to

Shrenik Punna

Resolution

False positive

Status

New

Select

Severity

Informational

Detect time

20:45:05

Detection name

Access from IP with bad reputation

Account name

skoenitzer.admin@ads.mhainc.com

Source endpoint IP address

20.127.44.172

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Shrenik Punna

Resolution

False positive

Status

New

Select

Severity

Informational

Detect time

20:39:10

Detection name

Unusual login to an endpoint

Account name

skoenitzer.admin

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-iis-rbt-p1.ads.mhainc.com

Assigned to

Shrenik Punna

Resolution

False positive

Status

New

Select

Severity

Informational

Detect time

19:21:10

Detection name

Unusual login to an endpoint

Account name

michaell

Account domain

ADS.MHAINC.COM

Source endpoint name

van-kskoka2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

18:55:10

Detection name

Unusual login to an endpoint

Account name

samuelc

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-dbohrer-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

18:43:10

Detection name

Unusual login to an endpoint

Account name

kimberly

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pvacca-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:46:10

Detection name

Unusual login to an endpoint

Account name

abioduna

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-kmclaug-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:23:10

Detection name

Unusual login to an endpoint

Account name

jeannem

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-cseast-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

16:19:10

Detection name

Unusual login to an endpoint

Account name

paulm

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dmathew-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:24:10

Detection name

Unusual login to an endpoint

Account name

stacis

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-lgelfan-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:18:07

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-wturner-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

15:13:10

Detection name

Unusual login to an endpoint

Account name

kevint

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-nkaprol-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:46:10

Detection name

Unusual login to an endpoint

Account name

robertn

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-mschube-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:37:05

Detection name

Access from IP with bad reputation

Account name

NKolla@mhainc.com

Source endpoint IP address

4.246.166.199

Country

United States

Destination application identifier

Windows Sign In

Source endpoint IP reputation

Hosting facility

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

14:09:10

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-alight2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:38:10

Detection name

Unusual login to an endpoint

Account name

MaryAnnT

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-eazis-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

13:08:04

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-ssmith-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:38:10

Detection name

Unusual login to an endpoint

Account name

AnthonyT

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-dcoste2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:14:10

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pdire-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

12:01:10

Detection name

Unusual login to an endpoint

Account name

RobertB

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-mshah-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:42:10

Detection name

Unusual login to an endpoint

Account name

dhanapals

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-raceced-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:37:10

Detection name

Unusual login to an endpoint

Account name

Ashley

Account domain

ADS.MHAINC.COM

Source endpoint name

ext-mtempl2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:22:10

Detection name

Unusual login to an endpoint

Account name

brianm

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-ckapfer-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:09:10

Detection name

Unusual login to an endpoint

Account name

sydneyb

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-tdeboe-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:06:10

Detection name

Unusual login to an endpoint

Account name

twinklep

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-mmcclu2-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

11:02:10

Detection name

Unusual login to an endpoint

Account name

dhanapals

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-jhurban-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

10:19:10

Detection name

Unusual login to an endpoint

Account name

michaeldi

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-felche1-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:48:11

Detection name

Unusual login to an endpoint

Account name

davidj

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-pdire-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:42:02

Detection name

Unusual login to an endpoint

Account name

johnb

Account domain

ADS.MHAINC.COM

Source endpoint name

phy-myannak-w10.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:38:03

Detection name

Unusual access to a user entity

Account name

dhanapals

Account domain

ADS.MHAINC.COM

Source endpoint name

PHY-DSELVA-W10

Destination SPN

mssqlsvc/aze-sql-ltcp-p1.ads.mhainc.com:1433

Destination endpoint

SRVFLO-SQL-LTCREP

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:33:10

Detection name

Unusual login to an endpoint

Account name

ceciliat

Account domain

ADS.MHAINC.COM

Source endpoint name

aze-sthod-tc1o6.ads.mhainc.com

Assigned to

Unassigned

Resolution

--

Status

New

Select

Severity

Informational

Detect time

09:29:04

Detection name

Anomalous certificate-based authentication (unusual TGS request)

Account name

shawnp

Account domain

ADS.MHAINC.COM

Source endpoint name

flo-spaik2-w10.ads.mhainc.com

Domain controller name

--

Assigned to

Unassigned

Resolution

--

Status

New