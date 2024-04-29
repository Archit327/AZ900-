
These role are applied to the falcon console users.
To create this falcon role one should have Falcon ADministrator role.

Falcon also works on Role based access control.

### Falcon RBAC
Basically consists of two elements:
- permissions- these are nothing but the checks whether the particular user trying to access something in portal does have the required acces to it or not. Permissions are just collected in permissions groups in falcon console such as prevention policies.
- roles- These are nothing but the collection of different permissons, so if the user wants certain level of access in the portal thne it will provided with appropriate role which has required permisssion to perform that task successfully.
  - default role - already built-in role like Flacon Administrator, these role are provided by the crowdstrike itself and cannot be updated.
  - custom role - the org can make role according to the permission or access they need to have for performing a certain task.

You can view all active roles in your CID in the Falcon console. 
Go to **Host setup and management > Falcon users > Roles and permissions**
Each role is listed with a description and this overview info:

- **Role type**: Identifies the role as a default role or a custom role
- **Permissions**: Number of permissions enabled for the role
- **Users**: Number of users assigned the role
- **Groups**: For Flight Control, the number of user groups and CID groups assigned the role

#### Create a custom role
1. Go to **Host setup and management > Falcon users > Roles and permissions** and click **Create role**.(You must have Falcon Administrator role to create new role)
2. Specify a role name and description, then click **Create role**.
3. Specify role permissions
4. Select the check box of permissions you want to enable and clear the check box of permissions you want to disable, then click **Save**.


### Default roles for falcon products

To view all the default roles 
**Host setup and management > Falcon users > Roles and permissions**

- **Falcon Console Guest:** (View-only) View documentation and your user profile. 
  Visit the CrowdStrike Customer Center to review knowledge base articles, read and subscribe to release notes and tech alerts, and use the chatbot for automated help.
  
- **Falcon Administrator:** Access all functionality in the console except certain Real Time Response (RTR) functionality.

- **Falcon Investigator:** Manage detections, search for events, and view exclusions. 
  View data about assets, accounts, and applications in asset management. 
  View Falcon Sandbox submissions and reports. View firewall rules, rule groups, policies, and firewall events. Download Forensics app. 
  Manage Identity Protection insights, incidents, reports, and threat hunter. 
  Create and run on-demand scans from the Falcon console.

- **Dashboard Admin:** Create, edit, manage, and delete dashboards. Create and edit shared tables.
  This role does not grant basic Falcon console access. 
  ==Users assigned this role must have at least one other role assigned to access the Falcon console.==

### switching between CIDs
User accounts use one set of credentials to access multiple CIDs. Once you’ve signed in to your home CID, you have automatic access to any other CIDs associated with your user account.

To change CIDs, on the navigation bar, click the current CID name and select the CID you want to access from the CID list. You can only work in one CID at a time. If you have multiple browser tabs open, they all update when you switch CIDs.


### Approved domains

Falcon uses your CID’s allowed domain list to add users. All user accounts that have access to your CID must be created with an email address from the list of approved domains. For example, if a user’s email address is user@local.com , the _@local.com_ domain must be registered in the CID’s domain allowlist.

**Note**: The domain allowlist is configured during a CID’s initial provisioning. To make changes to this list, contact Support.


### User Roles

- User roles determine what a user can see and do in the Falcon console. 
- Every Falcon user is required to have at least one role, which is assigned when a user account is created. 
- User roles are granted at the CID level, and you can have different roles in each CID you’re associated with. 
- In each CID, you have access to all of the features that your roles allow.

Falcon offers two types of user roles:
- global roles -  that are available across all products
- subscription level roles - that provide access to the features of a specific product.

These role identification will sum up that what is the purpose of that user in that subs. or environment, accordingly got the role.

### User Groups
We can also create user groups , such that taking a bunch of user and putting them in a group.
 These groups make it easier to manage access in complex environments by assigning a group of users to specific roles in specific child CIDs.


#### Creating a user in a standalone CID or FCTL child CID

1. On the **User management** page (**Host setup and management > Falcon users > User management**), click **Create User**.
2. Enter the user’s email address, first name, and last name. If you’re planning to enable single sign-on (SSO), the user’s email address must exactly match the information in your IdP.
3. Select one or more roles. You’ll see only the roles that are associated with the home CID’s purchased Falcon subscriptions, such as Falcon Prevent or Falcon Insight.
4. Click **Save**.
