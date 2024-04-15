
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


### Report details

In the **Report details** section, provide basic information that defines your report.

- **Name**: Provide a descriptive name for the scheduled report. The name identifies the report on the Scheduled reports page. It also appears in report notifications.
- **Description**: Enter an optional description to briefly define the report contents or purpose.
- **File format**: Choose a file format for generating your reports.
    - **Vulnerability management and host reports:** JSON (default) or CSV
    - **Dashboard reports:** PDF
    - **Cloud Security reports:** JSON or CSV (default)
    - **FileVantage reports:** JSON or CSV (default)
    - **Asset applications reports:** JSON or CSV (default)
- **Shared with**: Specify the users to share your report with. Users on this list are your account users that are entered in Falcon. You can share the report with as many users as you’d like. Your scheduled report will appear on shared users’ **Scheduled reports** page with options to view the report history and download generated reports.