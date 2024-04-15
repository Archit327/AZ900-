
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
1. Go to **Host setup and management > Falcon users > Roles and permissions** and click **Create role**.
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