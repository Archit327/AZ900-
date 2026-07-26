# Confidential PII/PHI Protection Plan

# 1. Objective

The objective is to protect sensitive PII and PHI data while ensuring business operations such as hiring workflows are not disrupted. The solution must balance strong data protection with ease of use for external recipients.

# 2. Business Context

GHR Healthcare shares candidate and employee information with external hospitals through multiple channels such as email, portals, and fax. These processes are time-sensitive, and previous security implementations caused delays and operational challenges.

# 3. Key Challenges and Considerations

·       External sharing across different systems (email, portals, fax)

·       Need for rapid document access by hospital staff

·       Maintaining security without disrupting hiring workflows

# 4. Proof of Concept (POC)

Implement Microsoft Purview Sensitivity Label "Confidential-PII/PHI" with a focus on secure yet seamless access.

**Solution approach:** The proposed proof of concept is to implement a Microsoft Purview sensitivity label named "Confidential - PII/PHI" and apply it to documents containing sensitive employment or healthcare-related information like SSN, Bank account number, Passport details, and intern IDs. The label will be configured so that protection travels with the document wherever it is shared.

## Deployment Phase

**Label Creation in Dev Environment**

**Secure Email Sharing**

·       Apply sensitivity label with encryption

·       Restrict access to intended recipients only with defined permissions as "view-only" or limited access controls like Edit, Forward or Export specific.

# 5. Permissions Strategy

·       Allow controlled viewing access

·       Restrict forwarding and unauthorized sharing

·       Limit copy/print/export where feasible (If required)

·       Avoid overly restrictive controls that impact usability

·       Secure sharing of sensitive data across organizations

·       Reduced risk of data leakage

# 6. Supported File types

The following file types can be labeled without encryption.

- **Adobe Portable Document Format:** .pdf
- **Microsoft Office Formats:  .**docx, .doc .xlsx, .xls, .xlsm .pptx, .ppt
- **Microsoft Project:** .mpp, .mpt
- **Microsoft Publisher:** .pub
- **Microsoft XPS:** .xps .oxps
- **Images:** .jpg, .jpe, .jpeg, .jif, .jfif, .jfi. png, .tif, .tiff (when put inside Doc or pdf)
- **Digital Negative:** .dng
- **Microsoft Office:** The following file types, including 97-2003 file formats and Office Open XML formats for Word, Excel, and PowerPoint.

# 7. Label Creation Approach

Step 1: Create Sensitivity Label

·       Create a label named "Confidential - PII/PHI" in Microsoft Purview.

Step 2: Configure Encryption & Permissions

·       Assign full control permissions to Compliance Manager or designated internal security group.

·       Define access for other users based on business needs:

o   External recipients: View-only or limited access

o    Internal users: Controlled access (view/edit based on role)

Step 3: Restrict Actions

·       Restrict forwarding of emails containing labeled documents

·       Limit copy, print, and export capabilities where feasible

Step 4: Assign Users and Groups

·       Configure permissions at user/group level

·       Include trusted hospital domains or specific recipients

Step 5: Publish Label Policy

·       Publish the sensitivity label to required users

·       Ensure visibility in Office apps (Outlook, Word, etc.)

# 8. Scenario under this label

The "Confidential - PII/PHI" sensitivity label is designed to protect documents under the following scenarios:  
  
**Use Case 1: Sensitivity Label Creation and Auto-Labelling**

In this scenario, the organization creates a **“Confidential - PII/PHI”** sensitivity label in Microsoft Purview and publishes it for users to apply on documents containing sensitive information..

Alongside manual usage, an auto-labelling policy is configured to automatically detect and classify PII/PHI data, ensuring consistent protection even when users do not explicitly apply the label.

**Use Case 2: Data Loss Prevention (DLP) for Protected Sharing**

In this scenario, once documents are labeled as **“Confidential - PII/PHI”**, a DLP policy governs how they are shared externally. The policy restricts sharing with unauthorized users or domains while allowing controlled access to approved recipients, ensuring sensitive data remains protected during external communication without disrupting business workflows.