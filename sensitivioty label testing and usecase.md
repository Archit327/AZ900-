

Persistent Protection of PII/PHI using Microsoft Purview
________________________________________
1. Use Case Overview
Title:
Implementation of Persistent Data Protection using Sensitivity Labels
Objective:
To ensure that sensitive data (PII/PHI) such as SSN, bank details, passport, etc.:
•	Is automatically classified and protected 
•	Remains protected even when shared externally 
•	Enforces access restrictions regardless of location (email, download, cloud upload) 
________________________________________
2. Solution Used
•	Microsoft Purview – Information Protection (Sensitivity Labels) 
________________________________________
 3. Label Configuration
Label Name: Confidential – Block PII/PHI
Key Configurations:
•	Assign Permissions and Access control configured (specific users only) with specific roles and responsibilities
•	Permissions assigned: For this usecase 
o	Viewer role
  
Applied Restrictions:
•	No Copy, No Print,  No Edit, No Forward, No Screenshot (within M365)
________________________________________
4. Core Concept (Important)
Sensitivity labels:
•	Apply classification + protection via label
•	Ensure protection travels with the document 
Which basically means that Even if file is:
•	Downloaded 
•	Shared externally 
•	Uploaded elsewhere 
It remains protected
________________________________________
5. Scenarios & Observations
Scenario 1: Stage 1
Internal Label Application
Action:
•	Document created with PII/PHI data 
•	Sensitivity label applied (auto labelling policy)
Outcome:
•	Restrictions enforced immediately 
•	Label visible in document 

 
________________________________________

Scenario 1: Stage 2
Sharing via Outlook (Internal → Internal Specific User)
Action:
•	Document shared via Outlook to same domain another user with Viewer rights
•	Access given to: johndoe@xyz.com 
Outcome:
•	Email inherits same protection 
•	Recipient can: ✔
o	View document 
•	Recipient cannot: ❌
•	Copy, Print, Edit, Forward, and screenshot.

 
Screenshot blacks out the word desktop app.

This is enforced by permissions and protection at OS level.
________________________________________
Scenario 3: Unauthorized External Access
Action:
•	Same document shared with another domain user (no permission) 
Outcome:
•	❌ Access denied 
•	❌ File cannot be opened 
Because access is identity-based
 
Scenario 4: Authorized External Access
Action:
•	External user explicitly granted access (johndoe@xyz.com with viewer permissions )
Outcome:
•	✔ User can open document 
•	✔ Can view only 
•	❌ Cannot copy / print / edit / share 
Restrictions remain enforced outside organization
Before applying the protection screenshot works
 
After applying label screenshot is blocked
 
Limitation- The screenshot blocking only works with the desktop application of word and outlook, not on browser.

________________________________________

Scenario 5: Sharing with Gmail User
Action:
•	Document shared to Gmail account 
Observed Behavior:
•	Email may land in Spam 
•	User must: 
o	Authenticate using Microsoft OTP / account as view once.
After Authentication:
•	✔ Can view document 
•	❌ Cannot copy / print / edit 
•	⚠ Screenshot possible (limitation since its open on browser) 
 
External users can access content via authentication 
________________________________________Scenario 6: Download Behavior
Action:
•	External user downloads document 
Outcome:
•	Requires authentication
 
•	File opens or appears only if: 
o	User has permission (it could be through domain or specific account)
 
Otherwise:
•	❌ Access denied 
 
Protection persists even after download
________________________________________
✅ Scenario 7: Upload to Google Drive / Other Platforms
Action:
•	Upload labeled document to Google Drive 
Outcome:
•	✔ File uploads successfully 
•	❌ File cannot be opened 
👉 Reason:
•	Encryption + label not compatible with non-Microsoft platforms 
________________________________________

6. Limitations Identified
7. Screenshot Limitation
•	Cannot fully prevent screenshots outside Microsoft apps 
•	OS-level limitation 
________________________________________
2. Google / Non-Microsoft Compatibility
•	Labeled files: 
o	Upload → ✔ 
User can be get the documents shared to personal gmail, but M365 label incompatibility with google WS would prevent opening it.
o	Open → ❌ 
Requires removing label before use.
________________________________________
3. External User Experience
•	Gmail users: 
o	Authentication required, but can view document with enough permissions.
o	May receive emails in spam 
________________________________________
4. No Control Over Manual Data Leakage
•	Users can: 
o	Re-type data 
o	Take photos on browsers.
________________________________________
7. Architecture Summary
Flow:
1.	Data identified (PII/PHI) 
2.	Label applied 
3.	Protection enforced 
4.	Access controlled via identity 
5.	Protection persists everywhere 
________________________________________
8. Key Capabilities Demonstrated
Capability	Result
Persistent protection	✅ Yes
External sharing control	✅ Yes
Identity-based access	✅ Yes
Encryption-based security	✅ Yes
Cross-platform protection	⚠ Partial

