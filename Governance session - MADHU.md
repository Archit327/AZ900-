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