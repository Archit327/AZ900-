
If we need to maintain eye on particular thing, like hosts then we can create scheduled reports which will provide time to time updates of that.

Create scheduled reports to get automatic, recurring updates of the data that matters most to you. You can download and share your scheduled reports, and receive a notification each time a new report is available.


For example, we can schedule reports for:
- A weekly summary of hosts in your environment
- critical vulnerabilities found in the hosts
- A monthly report of dashboard to give overview


### Default Roles required 

- **Scheduled Report Admin:** Can assign and revoke the Scheduled Report Analyst role, create scheduled reports, view and manage any scheduled report in the CID, and view, download, and delete any generated report in the CID.

- **Falcon Administrator and Intel Admin:** Can assign and revoke the Scheduled Report Admin role. They can also view and download reports generated from scheduled reports that have been shared with them.

- **Scheduled Report Analyst:** (we have this access only-view, we dont have permission for creating the reports)
  Can only access the Scheduled reports page and view and download generated reports of scheduled reports that have been specifically shared with them. You can assign this role to members who do not need any other Falcon access.

- **All other roles:** Can create scheduled reports, view and manage their own scheduled reports, and download and delete reports generated from their scheduled reports. They can also view and download reports generated from scheduled reports that have been shared with them.