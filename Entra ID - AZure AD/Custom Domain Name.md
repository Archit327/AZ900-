
## Creating Custom Domain NAme For AZure AD


- First of all sign in to the [Azure portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) with a global admin account for the directory.
- To add a new custom domain name, in the left pane, click **Custom domain names**.
- To add a custom domain click **Add custom domain**.

The next step is to add DNS entry for domain name in domain registrar. Once you add DNS entry, Azure AD can then verify that your organization owns the custom domain name.

Sign in to the domain name registrar for the domain. You can either use  an interface that lists all the tools for your hosting. Go to DNS tool and update the DNS zone file for the domain by adding the DNS entry provided to you by Azure AD.

Set the time to live (TTL) to 3600 seconds (60 minutes), and then save the record.

Once the DNS records are propagated, your domain name is ready to get verified. Click **Verify** to start the process.

Finally on the right pane we see verification succeeded message which means that our custom domain has been verified. On this page you may also download Azure AD connect.

==IMPORTANT==
Who can create a custom domain in Azure AD?
Only the user with Global ADmin role can create.

