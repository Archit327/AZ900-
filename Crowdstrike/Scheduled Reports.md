
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

- **Name**: The name identifies the report on the Scheduled reports page. It also appears in report notifications.
- **Description**: Enter an optional description to briefly define the report contents or purpose.
- **File format**: Choose a file format for generating your reports.
    - **Vulnerability management and host reports:** JSON (default) or CSV
    - **Dashboard reports:** PDF
    - **Cloud Security reports:** JSON or CSV (default)
    - **FileVantage reports:** JSON or CSV (default)
    - **Asset applications reports:** JSON or CSV (default)
- **Shared with**: Specify the users to share your report with. Users on this list are your account users that are entered in Falcon. You can share the report with as many users as you’d like. Your scheduled report will appear on shared users’ **Scheduled reports** page with options to view the report history and download generated reports.



#### Start date

The date to begin running the scheduled report. You can select the current date (default) or any date in the future. Falcon will generate a new report at the next scheduled interval following the designated **Start date**. It’s not possible to backdate the start of a report.

#### End date

The date to stop running the report. This is an optional field that you can use to schedule your report to run for a specific period of time only. For example, if you want to run your report for a four-month period, you can set an **End date** that occurs four months after the **Start date**.

Keep these considerations in mind when specifying an **End date**:
- After the **End date** is reached, no new reports are made for this scheduled report.
- The **End date** must be on or after the report’s **Start date**.
- The **End date** cannot be modified after a scheduled report is saved.
- If an **End date** is not specified, the report runs indefinitely, beginning on the **Start date**.

#### Frequency

The recurring interval at which the report will run. The frequency can be set to **Daily**, **Weekly**, or **Monthly**.

- Daily report: Generate reports on a daily basis at a designated time. You can schedule a report to run up to two times a day. For example, you can schedule a report to run every Friday at 8AM and 5PM.
- Weekly report: Generate reports weekly. You can select one or more days and up to two times to run the report each day. For example, you can schedule a report to run every Monday, Wednesday, and Saturday at midnight and 11AM.
- Monthly report: Generate a report monthly. You can select one or more days of the month and up to two times to run the report on those days. For example, you can schedule a report to run at 6AM on the 1st and 15th of each month.
- ==Note:== The monthly day selection is limited to days of the month that appear in every month (1st –28th).