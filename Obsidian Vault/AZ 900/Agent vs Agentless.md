Agent based and agentless security protection

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



