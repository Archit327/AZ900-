
Security groups are a way to collect user accounts, computer accounts, and other groups into manageable units.

### Working and use of Security Groups

SO basically in windows OS there are several built-in groups and accounts preconfigured with appropriate rights and permissions to perform specific task.

When we create a AD we create accounts that can be user accounts or computer accounts and also we create grps in which we will collect some accounts according to collective task they have to do.

Active Directory has two types of groups:

- **Security groups**: Use to assign permissions to resources.
- **Distribution groups**: Use to create email distribution lists.

### Security Grps

Security Grps can be provide an efficient way to assign access to resources on your network. 

- Assign user rights to security grps in AD -
  the user rights assigned to any security grp is by default inherited to the accounts or users added to that security grp.
  Grp policy can be used to assign user rights to security grps to delegate specific tasks
- Assigning permission to security grps for a resource -
  Permissions are little different form **user rights**. They are assigned to security grps for a shared resource, such that the permissions will determine who can access the resource and at what level i.e full access or just reading access.
  so for ex - if there are certain users in the security grp and they want access to certain resource, then they will provide permission directly at the grp level rather than individual level