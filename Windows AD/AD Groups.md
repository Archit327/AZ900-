
When we create a AD we create accounts that can be user accounts or computer accounts and also we create grps in which we will collect some accounts according to collective task they have to do.

So basically in windows OS there are several built-in groups and accounts preconfigured with appropriate rights and permissions to perform specific task.

AD has two types of grps 
- security grps - they are used to assign permission and user rights to a grp.
- distribution grps - they are used to create email distribution lists

## Security Grps
Security groups are a way to collect user accounts, computer accounts, and other groups into manageable units.

Security Grps can provide an efficient way to assign access to resources on your network. 

- Assign user rights to security grps in AD -
  the user rights assigned to any security grp is by default inherited to the accounts or users added to that security grp.
  Grp policy can be used to assign user rights to security grps to delegate specific tasks
- Assigning permission to security grps for a resource -
  This is different from user rights because user rights apply across an entire domain versus permissions that are directed to a specific resource. They are assigned to security grps for a shared resource, such that the permissions will determine who can access the resource and at what level i.e full access or just reading access.
  so for ex - if there are certain users in the security grp and they want access to certain resource, then they will provide permission directly at the grp level rather than individual level
  These security grps can be considered as Email entity. If you wanna try to sned email to everyone just use that security grp and then send the email to everyone in the grp.


## Distribution Grps
This is basically used for sending emails or msg to multiple users or accounts at the ssame time.


### Scope of Groups in AD

- Global 
- Universal 
- Domain local




| Scope        | Possiblemembers                                                                                                                                                                                                                                                                                                              | Scopeconversion                                                                                                                                                                                | Can grant permissions                                            | Possiblememberof                                                                                                                                                                               |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Universal    | Accounts from any domain in the same forest<br><br>Global groups from any domain in the same forest<br><br>Other Universal groups from any domain in the same forest                                                                                                                                                         | Can be converted to Domain Local scope if the group isn't a member of any other Universal group<br><br>Can be converted to Global scope if the group doesn't contain any other Universal group | On any domain in the same forest or trusting forests             | Other Universal groups in the same forest<br><br>Domain Local groups in the same forest or trusting forests<br><br>Local groups on computers in the same forest or trusting forests            |
| Global       | Accounts from the same domain<br><br>Other Global groups from the same domain                                                                                                                                                                                                                                                | Can be converted to Universal scope if the group isn't a member of any other Global group                                                                                                      | On any domain in the same forest, or trusting domains or forests | Universal groups from any domain in the same forest<br><br>Other Global groups from the same domain<br><br>Domain Local groups from any domain in the same forest, or from any trusting domain |
| Domain Local | Accounts from any domain or any trusted domain<br><br>Global groups from any domain or any trusted domain<br><br>Universal groups from any domain in the same forest<br><br>Other Domain Local groups from the same domain<br><br>Accounts, Global groups, and Universal groups from other forests and from external domains | Can be converted to Universal scope if the group doesn't contain any other Domain Local group                                                                                                  | Within the same domain                                           | Other Domain Local groups from the same domain<br><br>Local groups on computers in the same domain, excluding built-in groups that have well-known security identifiers (SIDs)                 |