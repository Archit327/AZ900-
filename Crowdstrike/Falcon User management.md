
This section we discuss about the creation of users and , giving roles to them and what are permissions required for the user and how to add.


### Who can create USER in falcon

- To create users and assign roles to them, you must have an administrative role, such as ==Falcon Administrator==.
- ==The Falcon Security Lead== can reset user passwords and MFA, but cannot manage users or user roles.

## Understanding user accounts

A user account contains attributes that identify a user and determine which resources they can access. It defines the user’s name, the unique email address that’s used to log in to Falcon, and the roles, groups, and CIDs the user is assigned to.

A unique user account is required for every person in your organization who needs access to the Falcon console. In a Flight Control environment, if a user requires access to more than one CID, you can associate the same user account with multiple CIDs in the Flight Control deployment.

### Unique email address

A user account is identified by its email address. When adding a new user account, the email address must be unique, it cannot be used by another user or added in another CID.

### Approved domains

Falcon uses your CID’s allowed domain list to add users. All user accounts that have access to your CID must be created with an email address from the list of approved domains. For example, if a user’s email address is user@example1.com , the _@example1.com_ domain must be registered in the CID’s domain allowlist.

**Note**: The domain allowlist is configured during a CID’s initial provisioning.

## Understanding user roles
- User roles determine what a user can see and do in the Falcon console. 
- Every Falcon user is required to have at least one role, which is assigned when a user account is created. 
- User roles are granted at the CID level, and you can have different roles in each CID you’re associated with. In each CID, you have access to all of the features that your roles allow.
- Falcon offers two types of user roles: a ==set of global roles== that are available across all products, and ==subscription-level roles== that provide access to the features of a specific product.
- You can see the roles assigned to a user on the **User management** page and can add and remove a user’s roles from the **User details** page. 
- Falcon Flight Control customers can also assign parent CID users additional roles through user groups. 
- These groups make it easier to manage access in complex environments by assigning a group of users to specific roles in specific child CIDs.


### Creating users

#### Creating a user in a standalone CID or FCTL child CID

1. On the **User management** page (**Host setup and management > Falcon users > User management**), click **Create User**.
2. Enter the user’s email address, first name, and last name. If you’re planning to enable single sign-on (SSO), the user’s email address must exactly match the information in your IdP.
3. Select one or more roles. You’ll see only the roles that are associated with the home CID’s purchased Falcon subscriptions, such as Falcon Prevent or Falcon Insight.
4. Click **Save**.

