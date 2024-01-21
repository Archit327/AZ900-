
### ==Introduction

Azure Monitor is a cloud based service provided by the Microsoft Azure which helps to monitor and manage the performance and health of the applications, infrastructure and networks running on the azure. 

Azure Monitor provides comprehensive monitoring capabilities for Azure network services , enabling us to proactively identify and address the potential issues.
In this document we will try to highlight the key considerations for configuring ==ALERTS, THRESHOLD , METRICS and FREQUENCY LIMITS== for optimal monitoring of the network resources.

#### Vocabulary 

1. ALERTS - These are the notifications that inform the conditions of the targeted or monitored resources and services.
2. METRICS - It is a feature of azure monitor that collects numeric data from monitored resources into a time-series database.
3. THRESHOLD - It is  a specific value which is spiked then the alert is send specifying that scenario or the change made in the particular monitored service or resource.
4. 

![[Pasted image 20240121153304.png]]

#### What are Metrics, Monitoring, and Alerting?

Metrics , Monitoring and Alerting are all inter related concepts that together form the basis of a monitoring system.
They have the ability to provide visibility into the health of your systems, help you understand trends in usage or behavior, and to understand the impact of changes you make.


#### What are Metrics ?
Metrics are represented as Raw Measurements of resources and usage or behavior that can be collected and evaluated through out the system.
They are like usage summaries provided by operating system, or they can be types of data tied to the specific functionality or work , for eg- how many login made in the VMs , or it can be the amount of data transferred or accessed .
Metrics can also display the data about the disk space, CPU load etc.

These metrics provide the raw material used by the monitoring systems to analyze the environment , automate responses and alert human being when required.

#### What is Monitoring ?


