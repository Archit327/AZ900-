
# 1. Use Case Overview

The customer operates a hybrid cloud environment where business data and sensitive information — including personally identifiable information (PII), financial records, and confidential documents — is stored across both Microsoft 365 (SharePoint, OneDrive, Exchange) and Google Workspace (Google Drive).

The organization already uses Microsoft Purview for data loss prevention within the Microsoft 365 ecosystem. The challenge is to extend that same DLP coverage to protect sensitive data stored in and shared through Google Drive, without deploying a separate, siloed security product.

## Business Problem

•         Sensitive data (PII, financial records, credentials, contracts) is stored in Google Drive, outside Microsoft's native protection boundary.

•         Users may upload corporate-sensitive files from managed devices to Google Drive, intentionally or accidentally.

•         Files already sitting in Google Drive may contain sensitive information that has never been scanned or flagged.

•         There is no current mechanism to alert, block, or audit these activities from a single compliance console.

•         The compliance team requires a unified alerting and policy management experience — not separate tools for Microsoft and Google.

## Goal

Extend Microsoft Purview DLP to cover Google Workspace so that:

1.       Sensitive information already stored in Google Drive is discovered and flagged.

2.       Future uploads of sensitive files from corporate endpoints to Google Drive are blocked or audited.

3.       Browser-based access to Google Drive on managed devices is monitored inline.

4.       All alerts and policy hits surface in the same Microsoft Purview compliance portal.

# 2. Available Approaches in Microsoft Purview

Microsoft Purview offers four distinct approaches to extend DLP coverage to Google Drive. Each operates at a different layer of the stack and serves a different purpose. Understanding all four is important before selecting the right combination.

## Approach 1 — Endpoint DLP with Sensitive Service Domain Groups

This approach uses Microsoft Defender for Endpoint (MDE) installed on Windows devices to monitor and block file upload activity at the endpoint level.

**How it works**

•         You define Google Drive (drive.google.com) as a Sensitive Service Domain Group in Purview Endpoint DLP settings.

•         A DLP policy is created with Devices as the location.

•         The policy rule triggers on the action: Upload to a restricted cloud service domain.

•         When a user attempts to upload a file containing sensitive information to Google Drive from a corporate device, the policy fires — auditing, warning, or blocking the upload.

**What it does NOT do**

This approach is purely activity-based and forward-looking. It does not scan files already stored in Google Drive. It only intercepts the moment a user attempts to upload from a managed Windows device.

**Prerequisites**

•         Microsoft Defender for Endpoint deployed on Windows endpoints (can be passive mode if another EDR is in use).

•         Microsoft 365 E5 Compliance or equivalent license.

## Approach 2 — Managed Cloud Apps via Microsoft Defender for Cloud Apps (MDCA)

This is the most comprehensive approach for protecting data already stored in Google Drive. It works by connecting Google Workspace as an OAuth-authenticated cloud app in Microsoft Defender for Cloud Apps (MDCA), giving Purview direct API-level visibility into the Google Drive environment.

**How it works**

•         Google Workspace is connected to MDCA via an OAuth app connector in the Defender portal.

•         Once connected, the Managed cloud apps location becomes available in Purview DLP policy creation.

•         A DLP policy targeting the Google Workspace instance will scan file content in Google Drive using Sensitive Information Types (SITs) or Sensitivity Labels.

•         Policy actions can include: audit only, alert, restrict sharing, or quarantine.

•         Purview creates a mirror policy in MDCA as an Information Protection File Policy that does the actual scanning.

**What it does cover**

•         Files already stored in Google Drive — scanned retrospectively after connector setup.

•         New files created or shared in Google Drive going forward.

•         Activity from any device, including unmanaged and personal devices.

**Prerequisites**

•         Microsoft Defender for Cloud Apps license (included in E5 Security and E5 Compliance).

•         Google Workspace Admin access to authorize the OAuth connector.

•         Microsoft 365 E5 Compliance license for full DLP scanning of third-party apps.

Important: E5 Security alone is NOT sufficient. Full DLP scanning for third-party connected apps requires E5 Compliance or the Microsoft 365 E5 bundle.

 

## Approach 3 — Inline Web Traffic via Edge for Business (Browser DLP)

This approach embeds DLP enforcement directly into the Microsoft Edge for Business browser on managed devices. It intercepts browser-based file uploads and paste/copy actions to Google Drive in real time.

**How it works**

•         A DLP policy is created in Purview with Inline web traffic as the location, selecting Edge for Business.

•         Purview communicates with the Microsoft Edge Management Service, which pushes configuration via Microsoft Intune.

•         When a user on a managed device attempts to upload a sensitive file to Google Drive in Edge for Business, the policy fires inline — before the upload completes.

**Prerequisites**

•         Devices must be enrolled in Microsoft Intune (MDM-managed).

•         Microsoft Edge for Business must be deployed as the corporate browser.

•         Microsoft 365 E5 Compliance license.

## Approach 4 — Network Data Security (SASE Integration)

This approach extends DLP to all HTTP/HTTPS web traffic at the network layer through integration with a Secure Access Service Edge (SASE) solution. It is the broadest coverage option but also the most complex and has additional cost implications.

**How it works**

•         Purview integrates with a SASE solution such as Microsoft Entra Internet Access, Netskope, or iboss.

•         Web traffic to Google Drive from any device on the corporate network (or connected via VPN/ZTNA) is inspected inline.

•         DLP policies with the Network data security location type apply to this traffic.

**Prerequisites**

•         A compatible SASE solution deployed and configured.

•         Pay-as-you-go billing enabled in Microsoft Purview (this is separate from E5 licensing and incurs consumption-based charges).

Note: This is the option indicated by the warning at the top of the Purview DLP locations page: 'Pay-as-you-go billing needs to be set up to configure policies for non-Microsoft 365 data sources.'

# 3.Effective Approach

Given the client's requirement to protect both existing data in Google Drive and prevent future leakage from corporate devices, a layered strategy using Approach 2 as the primary layer and Approach 1 as a secondary layer is recommended.

## Primary Layer — MDCA Connector (Approach 2)

This is the only approach that addresses the core problem: sensitive data already sitting in Google Drive. By connecting Google Workspace to MDCA and creating a Purview DLP policy targeting the Instances location, the compliance team gains:

•         Retroactive scanning of all existing Google Drive content.

•         Ongoing monitoring of new files and sharing activity.

•         Coverage of all users and all devices — not just managed Windows endpoints.

•         A unified alert experience in the Microsoft Purview compliance portal.

## Secondary Layer — Endpoint DLP (Approach 1)

Once the MDCA connector is in place, Endpoint DLP adds a strong preventative control specifically for corporate Windows devices. It stops sensitive files from being uploaded to Google Drive in the first place, catching the problem at the source before data ever leaves the device.

# 5. Implementation Guide — Primary Layer (MDCA + Purview DLP)

## Step 1: Connect Google Workspace to Microsoft Defender for Cloud Apps

·       Log in to the Microsoft Defender portal: https://security.microsoft.com

·       Navigate to Cloud Apps > Connected Apps > App Connectors.

·       Click + Connect an app and select Google Workspace.

·       Enter your Google Workspace domain and the admin account to be used for the OAuth connection.

·       Follow the OAuth consent flow — you will be redirected to a Google sign-in to authorise the connector.

·       Grant the required permissions (Drive, Gmail metadata, audit logs).

·       Return to the Defender portal and verify the connector shows a green/healthy status.

·       Allow 15–30 minutes for initial file activity ingestion.

Make sure the admin account used for the OAuth connection has Google Workspace Super Admin rights. Without this, the connector will connect but will have limited visibility.

## Step 2: Verify the 'Managed Cloud Apps' Location is Now Active

·       Log in to Microsoft Purview: https://purview.microsoft.com

·       Navigate to Data Loss Prevention > Policies.

·       Click + Create policy.

·       Proceed to the Locations step.

·       Confirm that Managed cloud apps is no longer greyed out and is now selectable.

If Managed cloud apps is still greyed out after connecting Google Workspace, verify that the connector status is healthy in Defender and that your Purview account has E5 Compliance licensing.

## Step 3: Create the DLP Policy for Google Workspace

·       In Purview DLP, click + Create policy. Choose Custom policy and click Next.

·       Give the policy a clear name (e.g., 'Google Drive — Sensitive Data Protection') and description. Click Next.

·       On the Locations page, deselect all locations except Managed cloud apps.

·       Under Managed cloud apps, click Choose instances and select your Google Workspace instance by the exact name you configured in Defender.

·       Click Next twice (leave Admin Units at default).

·       On the Policy settings page, click + Create rule.

·       Name the rule and configure Conditions: Content contains > Sensitive Information Types. Select the relevant SITs for your client (e.g., Credit Card Number, Passport Number, National ID).

·       Set instance count thresholds (e.g., trigger only if 5 or more instances are found) to reduce noise from incidental matches.

·       Under Actions, select Restrict third party apps and configure: Audit for initial rollout.

·       Configure user notifications and incident reports as needed.

·       Click Save and then Next.

·       IMPORTANT: On the Policy mode screen, select Run the policy in simulation mode. Do NOT set to On yet.

·       Review and click Submit.

## Step 4: Review in Simulation Mode (1–2 Weeks)

·       After 24–48 hours, go to Purview > Data Loss Prevention > Activity Explorer.

·       Filter by the new policy name. Review all matched files and locations.

·       Assess alert volume. If too high, tune the policy: increase instance count thresholds, increase confidence level requirements, or scope to specific shared drives.

·       Once alert volume is manageable and the policy is tuned, switch the policy from Simulation to On mode.

# 6. Implementation Guide — Secondary Layer (Endpoint DLP)

## Step 1: Configure Sensitive Service Domain Groups

·       In Microsoft Purview, navigate to Settings > Data Loss Prevention > Endpoint DLP Settings.

·       Scroll to Sensitive service domains and click + Create group.

·       Name the group (e.g., 'Unsanctioned Cloud Storage').

·       Add the following domains:

·       drive.google.com — Google Drive

·       *.dropbox.com — Dropbox

·       *.box.com — Box

·       onedrive.live.com — Personal OneDrive

·       Save the group.

## Step 2: Create the Endpoint DLP Policy

·       In Purview DLP, click + Create policy. Choose Custom policy.

·       Name it clearly (e.g., 'Endpoint — Block Upload to Unsanctioned Cloud').

·       On the Locations page, select Devices only. Leave all other locations deselected.

·       Create a rule with: Condition: Content contains > select your Sensitivity Labels (e.g., Confidential, Restricted) OR your Sensitive Information Types.

·       Under Actions, select Audit or restrict activities on devices.

·       Enable: Upload to a restricted cloud service domain.

·       Click + Choose different restrictions for sensitive service domain groups.

·       Select the group created in Step 1.

·       Set the action to Block with override (recommended) for initial rollout — this blocks the upload but allows users to justify a business override, providing both protection and an audit trail.

·       Configure incident reports and user notifications.

·       Start in Simulation mode first, then move to On after validation.

# 7. Additional Layers of Protection

The following approaches can be added on top of the primary and secondary layers to provide deeper, more comprehensive coverage. Each one closes a specific gap in the protection model.

## Layer 3 — Edge for Business Inline DLP (Browser Control)

When to add: When your managed devices use Microsoft Edge for Business as the corporate browser and are enrolled in Intune. This layer catches browser-based exfiltration that Endpoint DLP may miss, such as copy-paste operations within the browser.

**How to enable**

·       Ensure devices are enrolled in Microsoft Intune and Edge for Business is deployed.

·       In Purview DLP, create a new policy or edit an existing one.

·       On the Locations page, select Inline web traffic.

·       Under Inline web traffic, select Edge for Business.

·       Define rules targeting Google Drive URLs (drive.google.com) with upload or paste actions.

·       Save and deploy. Purview will automatically push configuration to Edge via Intune — no manual Intune policy authoring is required.

This layer also monitors paste-to-browser activity, which Endpoint DLP does not cover in all scenarios. It is particularly valuable for catching clipboard-based data exfiltration.

## Layer 4 — Network Data Security / SASE Integration

When to add: When the organisation needs DLP coverage for unmanaged devices, BYOD, or contractor machines that cannot have MDE installed. Also, valuable when the client has an existing SASE investment (e.g., Netskope, iboss, or Microsoft Entra Internet Access).

**How to enable**

5.       Enable Pay-as-you-go billing in Microsoft Purview (required for non-Microsoft 365 locations in network policies).

6.       Deploy and configure a compatible SASE solution as your network proxy.

7.       In the Defender portal, configure the SASE integration under Settings > Cloud Apps > Network integration.

8.       In Purview DLP, create a policy with the Network data security location.

9.       Define rules for Google Drive traffic and select appropriate actions.

Costs for Network Data Security are consumption-based (pay-as-you-go) and are separate from E5 licensing. Evaluate estimated traffic volume before enabling to avoid unexpected charges.

## Configure Google Workspace

As a Google Workspace Super Admin, perform these steps to prepare your environment.

1. Sign in to the [Google Workspace](https://console.cloud.google.com/) as a Super Admin.
2. Create a new project named **Defender for Cloud Apps**.
3. Copy the **Project number**. You'll need it later.
4. Enable the following APIs:

- Admin SDK API
- Google Drive API

6. Create Credentials for a service account with the following details:

- Name: _Defender for Cloud Apps_
- Description: _API connector from Defender for Cloud Apps to a Google workspace account_.

6. Grant this service account access to the project.
7. Copy the following information of the service account. You'll need it later

- Email
- Client ID

9. Create a new key. Download and save the file and the password required to use the file.
10. In the API controls, add a new Client ID in the Domain Wide Delegation, using the Client ID you copied above.
11. Add the following authorizations. Enter the following list of required scopes (copy the text and paste it in the **OAuth Scopes** box):

**txt**

https://www.googleapis.com/auth/admin.reports.audit.readonly,https://www.googleapis.com/auth/admin.reports.usage.readonly,https://www.googleapis.com/auth/drive,https://www.googleapis.com/auth/drive.appdata,https://www.googleapis.com/auth/drive.apps.readonly,https://www.googleapis.com/auth/drive.file,https://www.googleapis.com/auth/drive.metadata.readonly,https://www.googleapis.com/auth/drive.readonly,https://www.googleapis.com/auth/drive.scripts,https://www.googleapis.com/auth/admin.directory.user.readonly,https://www.googleapis.com/auth/admin.directory.user.security,https://www.googleapis.com/auth/admin.directory.user.alias,https://www.googleapis.com/auth/admin.directory.orgunit,https://www.googleapis.com/auth/admin.directory.notifications,https://www.googleapis.com/auth/admin.directory.group.member,https://www.googleapis.com/auth/admin.directory.group,https://www.googleapis.com/auth/admin.directory.device.mobile.action,https://www.googleapis.com/auth/admin.directory.device.mobile,https://www.googleapis.com/auth/admin.directory.user   ```

In the Google admin console, enable the service status for  Google Drive for the Super Admin user that will be used for the connector. We recommend that you enable the service status for all users.