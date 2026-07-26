
# Introduction

Microsoft Purview and Microsoft Priva provide a comprehensive set of tools to help organizations **secure, govern, and protect their data** while ensuring privacy and regulatory compliance. However, to access the full range of features, organizations must have the appropriate **Microsoft 365 licensing**.

This document outlines the **required licenses** to leverage all features within **Microsoft Purview (Data Security, Compliance, and Governance)** and **Microsoft Priva (Privacy Risk Management and Subject Rights Requests)**, along with the different options available based on an organization’s existing Microsoft 365 subscription.

# Microsoft Purview & Priva – Licensing & Dependency Matrix

## 1. Core Licensing Comparison (Purview & Priva Capabilities)

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Solution Area**|**Capability / Feature**|**M365 E3 (+ EMS E5)**|**M365 E5**|**E5 Compliance Add-on**|**Notes**|
|**Data Discovery & Classification**|**Sensitive Info Types (built-in)**|**✅ Basic**|**✅ Advanced**|**✅ Advanced**|**E3 supports basic SITs; E5 expands with accuracy & classifiers**|
||**Custom Sensitive Info Types**|**✅**|**✅**|**✅**|**Available across all, but more powerful with E5**|
||**Trainable Classifiers**|**❌**|**✅**|**✅**|**Requires E5 or Compliance add-on**|
||**Data Explorer**|**⚠️ Limited**|**✅**|**✅**|**Full visibility requires E5**|
||**Activity Explorer**|**⚠️ Limited**|**✅**|**✅**|**Enhanced insights with E5**|
|**Sensitivity Labeling**|**Manual Labeling**|**✅**|**✅**|**✅**|**Available across all licenses**|
||**Auto-labeling (content-based)**|**❌**|**✅**|**✅**|**Key E5 capability**|
||**Auto-labeling (trainable classifiers)**|**❌**|**✅**|**✅**|**Advanced automation**|
|**Data Loss Prevention (DLP)**|**M365 DLP (Exchange, SharePoint, OneDrive)**|**✅**|**✅**|**✅**|**Core DLP available in E3**|
||**Endpoint DLP**|**❌**|**✅**|**✅**|**Requires Defender + E5**|
||**Teams DLP**|**⚠️ Limited**|**✅**|**✅**|**Full coverage with E5**|
||**Adaptive DLP (insider risk-based)**|**❌**|**✅**|**✅**|**Advanced scenario**|
|**Insider Risk Management**|**Insider Risk Policies**|**❌**|**✅**|**✅**|**E5 or add-on required**|
||**Risky User Detection**|**❌**|**✅**|**✅**|**Behavioral analytics**|
|**Communication Compliance**|**Policy creation & monitoring**|**❌**|**✅**|**✅**|**Not available in E3**|
||**AI-based detection (toxicity, etc.)**|**❌**|**✅**|**✅**|**Advanced models**|
|**eDiscovery**|**eDiscovery (Standard)**|**✅**|**✅**|**✅**|**Included in E3**|
||**eDiscovery (Premium)**|**❌**|**✅**|**✅**|**Advanced workflows, review sets**|
||**Litigation Hold**|**✅**|**✅**|**✅**|**Available across licenses**|
|**Audit**|**Audit (Standard)**|**✅**|**✅**|**✅**|**Basic logs**|
||**Audit (Premium - long retention, high-value events)**|**❌**|**✅**|**✅**|**Critical for investigations**|
|**Data Lifecycle Management**|**Retention Labels/Policies**|**✅**|**✅**|**✅**|**Core feature**|
||**Auto-apply retention labels**|**❌**|**✅**|**✅**|**Requires E5**|
|**Compliance Manager**|**Assessments (e.g., HIPAA)**|**✅**|**✅**|**✅**|**Available across licenses**|
||**Automated improvement tracking**|**⚠️ Limited**|**✅**|**✅**|**Better insights in E5**|
|**Priva (Privacy Management)**|**Priva Subject Rights Requests**|**❌**|**✅**|**✅**|**Requires E5 or add-on**|
||**Priva Risk Management**|**❌**|**✅**|**✅**|**Privacy risk insights**|
||**Data Minimization & Insights**|**❌**|**✅**|**✅**|**Advanced privacy controls**|
|**Information Barriers**|**Segmentation policies**|**❌**|**✅**|**✅**|**E5 required**|
|**Records Management**|**Basic Records Management**|**⚠️ Limited**|**✅**|**✅**|**Advanced features in E5**|

## 2. Pay-As-You-Go (PAYG) / Azure Subscription Requirements

Certain **advanced Purview capabilities require Azure PAYG billing integration**:

|   |   |   |
|---|---|---|
|**Solution / Feature**|**PAYG Required?**|**Details / When Required**|
|**Microsoft Purview (Data Governance – Unified Catalog)**|✅ Yes|For scanning non-M365 data sources (Azure, SQL, on-prem, multi-cloud)|
|**Priva (Privacy Management)**|⚠️ Partial|Some features may require PAYG depending on usage scale|
|**Audit (Premium – Extended Retention)**|⚠️ Conditional|Additional storage beyond default limits|
|**Communication Compliance (Advanced AI models)**|⚠️ Conditional|AI processing at scale may require backend billing|
|**Insider Risk Management (Advanced analytics)**|⚠️ Conditional|Depends on data volume and signals|
|**Auto-labeling at scale**|⚠️ Conditional|Large-scale processing may require backend compute|
|**Data Lifecycle (event-based, large datasets)**|⚠️ Conditional|Storage/processing overhead|
|**AI Integration (Copilot, third-party connectors)**|✅ Yes|Required for metered services and AI processing workloads|

# **Licensing Requirements for Microsoft Purview & Microsoft Priva**

## **1. Organizations with Microsoft 365 E5** – **Best Option – No Additional Licensing Required**

If an organization has **Microsoft 365 E5**, they already have access to **all Microsoft Purview and Microsoft Priva features**, including:

- **Microsoft Purview Data Security** (Information Protection, Auto-labeling, DLP, Insider Risk, Encryption)
- **Microsoft Purview Compliance** (eDiscovery, Audit, Communication Compliance, Data Lifecycle Management)
- **Microsoft Purview Governance** (Records Management, Data Loss Prevention, Sensitivity Labels)
- **Microsoft Priva** (Privacy Risk Management & Subject Rights Requests)

No additional licenses or add-ons are required to use all available capabilities.

## **2. Organizations with Microsoft 365 E3 – Add-On Licensing Options**

If an organization has **Microsoft 365 E3**, they have the following options to **enhance security, compliance, and privacy features**:

#### **Option 1: Add Enterprise Mobility + Security (EMS) E5 – $16.40/user/month**

**Best for:** Organizations with **Microsoft 365 E3** looking to **enhance security and compliance** without upgrading to Microsoft 365 E5.

**Includes:**

- **Microsoft Purview Information Protection** (Auto-labeling, DLP, Encryption)
- **Microsoft Purview Insider Risk Management**
- **Microsoft Priva Privacy Risk Management & Subject Rights Requests**

#### **Option 2: Add Microsoft 365 E5 Compliance – $12.00/user/month**

**Best for:** Organizations that need **advanced compliance solutions** without full security enhancements.

**Includes:**

- **Microsoft Purview Advanced Compliance Features** (eDiscovery, Insider Risk, Communication Compliance)
- **Microsoft Purview Data Lifecycle Management**
- **Microsoft Priva Subject Rights Requests**

#### **Option 3: Upgrade to Microsoft 365 E5 (No Teams) – $54.75/user/month**

**Best for:** Organizations seeking a **fully integrated solution** covering **security, compliance, governance, analytics, and privacy**.

**Includes:**

- **All Microsoft Purview & Priva solutions.**
- **Advanced security** (Defender for Office 365, Azure AD P2).
- **Compliance, Insider Risk, eDiscovery, and more.**

# **PAYG Considerations**

- Required when:

- Extending beyond M365 (e.g., **data estate scanning**)
- Using **AI-driven or compute-heavy features**
- Scaling **compliance operations across large datasets**

---

## Pay as you go - Data security

|   |   |   |   |
|---|---|---|---|
|**Solution**|**What It Does**|**Billing Unit**|**How Billing Works**|
|**In-Transit Protection**|Protects data moving across web/apps|$0.50 Per 10K requests|Charged based on number of network requests analyzed|
|**At-Rest Protection**|Assets like files, tables, resource sets|**$0.0165** per asset per day or  <br>~**$0.50** per month|Billing is calculated based upon the number of assets at rest that are auto-labeled and protected under these policies.|
|**Insider Risk Management (PAYG)**|Detect risky user behavior (AI + signals)|$25 per DSPU (processing units)|1 DSPU = processing ~10,000 activity logs|
|**Data Security Investigations**|AI-based deep investigation of data exposure|$5/Compute Unit/hour<br><br>$5/GB/Month|Compute Units + Storage (GB)<br><br>Pay for compute used + data stored|
|**OCR - Image Extraction (for classification)**|Extracts images for analysis|Per image<br><br>$1/1000 Images included quantity|First ~2,500 images/month free, then billed|
|**On-Demand Classification**|Scans and Classify file that are never classified|$20/10K Assets been scanned or classified|Use in-product cost estimate tool before initiating the scans.|

Billing is based on two meters:

1.      Data Security Investigations Storage Meter – For storing investigation-related data, charged by GB

2.      Data Security Investigations Compute Meter – For the computational capacity required to perform AI‑powered data analysis and execute investigation actions on data, charged by Compute Units (CUs).

Monthly charges are estimated from the amount of data stored and the number of CUs consumed per hour. This pay-as-you-go model ensures customers only pay for what they need, when they need it.

Key Glossary

·       A request - one interaction with a service/app that gets inspected. Every time data is sent, uploaded, or accessed, that’s a request. It’s based on actions/events

Example:

User uploads 1 file → 1 request

User uploads 100 files → 100 requests

·       DSPU – Data security Processing Unit  
It’s basically how much data/activity Microsoft processes for risk analysis. Think of it as batch of user activity logs being analyzed.  
Billing is calculated based on the number of processing units required for the indicators and users selected in the policies. A DSPU is defined as the compute required to process 10,000 user activity logs.

---

## Data Compliance (AI + Communication + Records)

This is where **modern PAYG metering is heavily used**, especially for AI and communications.

|   |   |   |   |
|---|---|---|---|
|**Solution**|**What It Does**|**Billing Unit**|**How Billing Works**|
|**Audit (for non-M365 / AI apps)**|Tracks activity (e.g., ChatGPT usage)|$0.015 Per 1k audit records|Charged for logs ingested|
|**Audit (all Microsoft Applications)**|Microsoft Copilot, Security Copilot, custom application built using copilot studio or Azure Ai Studio|$15 per 1M records|Log retention for 180 days|
|**Communication Compliance**|Monitors messages (Teams, chats, etc.)|Standard - **$0.30** per 1K text records<br><br>Premium - **$0.50** per 1K text records|Charged per 1k text records analyzed|
|**Data Lifecycle Management Premium**|Retention for messages/data|$0.0002 per 1K text messages stored per day<br><br>Estimated - $6 per 1M text message|Charged based on text message stored or retained|
|**eDiscovery Premium**|Legal investigations|$0.6667 per GB per day or  <br>~$20.01 per GB per month|Charged based on stored case data|

Key Glossary:

-          A text record is up to 1,000 characters. If a single prompt/response is more than 1,000 characters, it becomes multiple text records based on length.  
Messages exchanged within Microsoft 365 applications continue to be covered under the E5 subscription. Non-Microsoft 365 generative AI interactions are covered under the pricing below.

## Azure Purview (Classic)

This is the **older model** (pre-2025), still relevant for some customers.

|   |   |   |   |
|---|---|---|---|
|**Solution**|**What It Does**|**Billing Model**|**Notes**|
|**Data Map**|Metadata storage|**$0.63** per 1 vCore Hour|Core catalog engine.<br><br>Power Bi and SQL Server are free for limited time|
|**Data Estate Insights generation**|Reporting & analytics|**$0.82** per 1 vCore Hour|Based on usage|
|**Data Map Consumption Scanning (data sources)**|Scan Azure, SQL, etc.|**$0.411** per Capacity Unit per Hour|Based on scanning activity and size of asses and entity|

By default, a Microsoft Purview account is provisioned with a Data Map of at least 1 Capacity Unit. 1 Capacity Unit supports requests of up to 25 data map operations per second and includes storage of up to 10 GB of metadata about data assets. The first 1 MB of Data Map meta data storage is free for all customers.

A data map operation is a create, read, update, or delete of an entity in the Data Map. Examples of an entity include a data asset or a lineage relationship between two data assets. A search request may require multiple operations depending on the assets returned and complexity of the request. The storage size of an entity may vary depending on the type of entity and annotations associated with the entity.

Data Map requires an additional Capacity Unit for every 10 GB of metadata storage required. For example, a Data Map with 10 GB of metadata storage is billed at 1 Capacity Unit per hour. If the addition of new data assets increases the size to 10.1 GB, the Data Map is billed at 2 Capacity Unit per hour.

## Data Governance (Unified Catalog & Data Map)

|   |   |   |   |
|---|---|---|---|
|**Solution**|**What It Does**|**Billing Unit**|**How Billing Works**|
|**Unified Data Catalog**|Organizes & governs data assets|Per governed asset/day|Only assets linked to governance concepts are billed|
|**Data Map (metadata scanning)**|Scans and maps data sources|Often free (conditions apply)|No charge unless governance is applied|
|**Data Quality / Health Management**|Runs data quality checks|DGPU (processing unit)|Charged per compute used per run|

Data management usage is billed based on the Data Governance Processing Unit (DGPU) pay-as-you-go meters. A DGPU (1 DGPU) is the equivalent to 60 minutes of fully-managed compute time, taken to produce data management results. DGPU is available in three different performance options: Basic, standard, and advanced. By default any data management rule or health control is run on the Basic SKU. A customer can switch SKU’s based on the speed of compute suitable for their organization. 

For example, if a customer runs 100 Data Management rules and controls in a single day, and each run produces 0.02 DGPU with the Basic SKU, then the total DGPU for that day would equal two DGPU, costing the customer $30. See the table below for pricing for Data Health Management feature by SKU in US East pricing.

|   |   |   |
|---|---|---|
|**Feature**|**SKU**|**Pay-As-You-Go Price**|
|Unified Catalog|Basic|**$15** per Data Governance Processing Unit|
|Standard|**$60** per Data Governance Processing Unit|
|Advanced|**$240** per Data Governance Processing Unit|
