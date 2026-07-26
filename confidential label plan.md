# Confidential PII/PHI Protection Plan

# 1. Objective

The objective is to protect sensitive PII and PHI data while ensuring business operations such as hiring workflows are not disrupted. The solution must balance strong data protection with ease of use for external recipients.

# 2. Business Context

GHR Healthcare shares candidate and employee information with external hospitals through multiple channels such as email, portals, and fax. These processes are time-sensitive, and previous security implementations caused delays and operational challenges.

# 3. Key Challenges and Considerations

·       External sharing across different systems (email, portals, fax)

·       Need for rapid document access by hospital staff

·       Maintaining security without disrupting hiring workflows

# 4. Proposed Solution

Implement Microsoft Purview Sensitivity Label "Confidential-PII/PHI" with a focus on secure yet seamless access.

# 5. Solution Approach (Phased)

**Phase 1: Secure Email Sharing**

·       Apply sensitivity label with encryption

·       Restrict access to intended recipients only

·       Enable "view-only" or limited access controls

5.1 Later Phases-  
Phase 2: External Portals and File Sharing

·       Extend controls to third-party platforms (e.g., Google Drive)

·       Monitor and restrict sensitive uploads where possible

# 6. Permissions Strategy

·       Allow controlled viewing access

·       Restrict forwarding and unauthorized sharing

·       Limit copy/print/export where feasible (If required)

·       Avoid overly restrictive controls that impact usability

# 7. Expected Outcomes

·       Secure sharing of sensitive data across organizations

·       Reduced risk of data leakage

# 8. Label Creation Approach

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

Step 6: User Adoption

·       Train staff to apply the label before sharing documents

·       Focus on email-based sharing as the initial rollout

# 9. Scenario under this label

The "Confidential - PII/PHI" sensitivity label is designed to protect documents under the following scenarios:  
  
**Scenario 1: Sharing Candidate Packets via Email**

·       Documents containing PII (e.g., Social Security Numbers) or PHI are shared with hospital hiring managers.

·       Only intended recipients can access the document.

·       Unauthorized forwarding or redistribution is restricted.

**Scenario 2: Cross-Organization Sharing**

·       Documents are shared across different organizations (cross-tenant).

·       Protection remains with the document regardless of where it is opened.

**Scenario 3: Internal Access Control**

·       Compliance Managers or designated security teams have full control over sensitive documents.

·       Internal users have role-based access (view/edit as required).

**Scenario 4: Preventing Data Leakage**

·       Restrict copying, printing, exporting, and forwarding where feasible.

·       Ensure sensitive data is not misused or shared beyond intended recipients.

**Scenario 5: Multi-Channel Sharing Considerations**

·       - Email: Fully protected using Purview encryption and labeling.

·       Portals/Drives: Partial control through monitoring and restrictions.

·       Fax: Out of scope (physical transmission cannot be protected digitally).