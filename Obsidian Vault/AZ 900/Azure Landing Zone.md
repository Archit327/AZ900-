

the term landing simply defines a zone or a  architecture which shows that how a framework of their cloud should look like.

it is an environment that follows key design principles in different design areas.
these design principle accommodate all application portfolios AND enable application migration , innovation and modernization at scale.

azure landing zone uses substcription to isolate application resrces and platform resrces .
subscription for application resrces are called as application landing zoneand subscription for platform resrces is called as platform landing zone.

when we plan for LZ
=========================

when we define the strtegy and list the requirement and understand the workload and then we plan building the landing zone accordingly.
landing zone architecture basically depends upon the needs and of the customer.

Migration Landing zone : 8 key design Principles
=



![[ns-arch-cust-expanded.svg]]


### STEP 1--> select what type of environment u want to build

![[Pasted image 20231129160004.png]]

***Landing zone*** is the environment which is provisioned to host the workloads that are migrated from on premises environment to the azure

***cloud adoption framework migrate landing zone blueprint*** creates a landing zone which can updated to meet specific needs.

![[Pasted image 20231129161409.png]]


### step 2

ADD compliance
=
![[Pasted image 20231129161542.png]]


there are more than 30 compliance standards which can be applied accordingly , in order to obey best practises.


### step 3
deployment tools/scripts
=

now we can select which type of scripts we need to deploy
1. Terraform scripts
2. azure ARM templates
3. Azure blueprint

WORKFLOW
=
![[Pasted image 20231129162214.png]]


APPROACH
=

#### 1. PREREQUISITES CHECK
#### 2. CAF LANDING ZONE WORKSHOP
#### 3. LANDING ZONE SCRIPTS, DEPLOYMENT AND TEST
#### 4. DOCUMENTATION AND TRAINING



AZure Landing Zone Bicep
=

So basically ALZ used the ARM templates for infrastructure provisioning .
ARM template are basically the JSON scripted code files that the describes the resources needed to deploy an application in azure.

AZURE BICEP on the other hand is a domain Specific language for deploying azure resource declaratively.
it can simplify the creation and management of the resources by providing more concise and readable syntax compared to json.


Foundation level landing zone
=

#### Scope
it is designed to provide a baseline environment for the initial setup of workloads in azure. 
it simply focuses on establishing core components and networking foudations.

#### Purpose
the primary goal of foundation landing zone is to create a secure, compliant  and well architected environment that can serve as the basis for deploying specific workloads or apploication.

#### components
it includes the cire infrastructure components like network, identity , and security.
also policy for governance and resource organisation principles

Enterprise level landing zone
=

#### scope
it extends the concept of foundation landing zoneto cover broader aspects of an org's cloud strategy.
it involves consideration for multiple business units - applications, and diverse workloads.
#### Purpose:
The main purpose of an enterprise landing zone is to scale the landing zone approach to accommodate the complexities of larger organizations with diverse requirements. It addresses governance, security, and compliance needs across various business units and applications.
#### Components:
Includes components from the foundation landing zone and expands to cover additional aspects such as multi-environment strategies, cross-region considerations, hybrid cloud scenarios, and advanced networking and security configurations.


Advance level landing zone
=

 **Advanced Networking:**
     Explore advanced networking features such as Azure Virtual WAN, global VNet peering, and Azure Front Door for more sophisticated traffic routing and optimization.
 
 **Advanced Security:**
    -  Implement advanced threat protection and security solutions like Azure Sentinel for advanced security information and event management (SIEM).

 **Multi-Cloud and Hybrid Strategies:**
    AZURE ARC and AZURE API  management
    - Consider advanced scenarios where workloads span multiple cloud providers or involve complex hybrid cloud architectures.

. **High-Performance Computing (HPC):**
    azure hpc and azure cyclecloud
    - If applicable, design the landing zone to support high-performance computing workloads using specialized Azure services and configurations.

 **Advanced DevOps and CI/CD:**
    azure artifacts and 
    azure devops with Yaml pipelines
    
 **AI and Machine Learning (ML) Integration:**
     * Azure ML and azure cognitive services
    - If your organization is focused on AI and ML, configure the landing zone to support advanced analytics and machine learning workloads, utilizing services like Azure Machine Learning.




 **Serverless Architectures:**
     azure logic apps with durable functions
     azure event grid
    
 **Distributed Systems and Microservices:**
     Design the landing zone to support distributed systems and microservices architectures using Azure Kubernetes Service (AKS) or Azure Service Fabric.
     
 **Advanced Monitoring and Management:**
    Implement advanced monitoring and management solutions with Azure Monitor, Azure Policy, and Azure Automation for more sophisticated insights and automation.
    
1. **Cost Optimization Strategies:**
    
    - Utilize advanced cost management strategies, leveraging reserved instances, spot instances, and advanced Azure Cost Management features.
11. **Data Analytics and Big Data:**
    
    - If your organization deals with large-scale data analytics, configure the landing zone to support big data workloads using services like Azure Databricks, Azure Synapse Analytics, or HDInsight.



Custom landing zone
=

**Organizational Policies and Standards:**
security and access controll
network architecture
resource hierarchy and tagging
integration with existing system
compliance and governance framework
identity and authentication
high availability and 

FOUINDATION  LANDING ZONE

| Governance           | Networking | Identity            | Security |
| -------------------- | ---------- | ------------------- | -------- |
| RBAC                 | vnet       | entraID             |          |
| Policy               | subnets    | managed identities  |          |
| resource lock        | peerings   | external identities |          |
| tags                 | nsg,asg    |                     |          |
| MG,Subs,RG,Resources |            |                     |          |
| network watcher      |            |                     |          |
|                      |            |                     |          |

Enterprise landing zone

| Governance           | Networking       | Identity            | Security           |
| -------------------- | ---------------- | ------------------- | ------------------ |
| RBAC                 | vnet             | entraID             | sentinel           |
| Policy               | subnets          | managed identities  | defender for cloud |
| resource lock        | peerings         | external identities | ddos               |
| monitor              | nsg,asg          | MFA                 | application GW     |
| blueprints           | Firewall         | SSO                 | KEY VAULT          |
| MG,Subs,RG,Resources | VPN GW           | entra connect       |                    |
| tags                 | Bastion          | conditional access  |                    |
| cost managements     | NAT              | AD Roles            |                    |
| log analytics        | express route GW | PIM                 |                    |




![[Pasted image 20231205135812.png]]