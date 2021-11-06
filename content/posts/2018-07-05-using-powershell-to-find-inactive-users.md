+++
title = "Using PowerShell to Find Inactive Users in AD and Exchange"
date = 2018-07-05
tags = [ "active directory", "exchange", "powershell" ]
+++

Recently, I was working with a client to consolidate of several Active Directory and LDAP domains into a single unified domain. To ensure we were starting with a _clean slate_, one of the first steps I performed was an audit to find any inactive accounts in the Active Directory domains. This allowed the client to investigate and/or remove any stale accounts prior to consolidation. There are many professional (and possibly expensive) tools which can be used to automate this process, but for small to mid-sized domains, this can be done fairly easily using the following __PowerShell__ command and some __Excel__ manipulation.

```powershell
Get-AdUser -SearchBase "ou=users,dc=company,dc=com" -Filter * -Properties * | select SamAccountName, distinguishedName, LastLogonDate, Department, Title, Enabled | Export-Csv C:\Temp\AdUserLastLogonDate.csv
```

In this example, I have focused on the the __users__ OU, although this script could be used to investigate stale computer or service accounts as well. While I only needed the user's account name and _LastLogonDate_ in this report, I also included fields such as _Department_ and _Title_ to assist in identifying the users. The CSV file produced by __PowerShell__ could then be opened in __Excel__ to quickly locate any inactive accounts by sorting on the _LastLogonDate_ field. That said, there are some specific issues to be aware of using this process.

1. The _LastLogonDate_ attribute can be out by up to 14 days (due to how this field is replicated in AD).
2. If the _LastLogonDate_ is blank, this indicates the account has never logged in.

With the information provided in this report, I was able to work with the client to identify stale accounts which could be removed or further investigated. In at least one case, using this report the client identified a service which had not been working in months and was only brought to their attention when the service account showed up as _inactive_.

Once this was done, the client decided to perform a similar audit of user mailboxes in Exchange. As above, this can be done fairly easily with a few __PowerShell__ commands and __Excel__.

```powershell
$UserCredential = Get-Credential
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $UserCredential -Authentication Basic -AllowRedirection
Get-mailbox -OrganizationalUnit "ou=users,dc=company,dc=com" -resultsize unlimited | Get-MailboxStatistics | select DisplayName, TotalItemSize, Itemcount, legacyDN, lastlogontime | Export-Csv C:\Temp\ExchangeMBOXReport.csv
```

In this example, the client mailboxes are hosted on __Office 365__. The first two commands are required to authenticate and connect to the __Exchange Online PowerShell__ interface. Once connected, the `Get-Mailbox` command is used to export all user mailboxes, and filtered through the `Get-MailboxStatistics` command, giving the client a quick and dirty report of mailbox usage as well. Once again, the CSV file produced by __PowerShell__ could be opened and manipulated in __Excel__ to quickly locate any inactive mailboxes by sorting on the _lastlogontime_ field.

Unfortunately, in more complex environments where __Blackberry BES__ (yes, that is still being used out there) or some other Server-Side Antivirus tools, the _lastlogontime_ field may not be correct, as this date is updated every time the mailbox is scanned. In these cases, you will need to use a different process to identify _inactive_ mailboxes. One possible way is to report based on the date of the last item in the _Sent Items_ folder. Rather than get into the details here, I'll refer you to this [article](https://gallery.technet.microsoft.com/scriptcenter/List-Inactive-Mailboxes-on-1ac82ddf) on the __Microsoft Script Center__ which covers the process in depth.

As always, I hope you found this post useful, or at least interesting. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/bftsystems).

Thanks for reading.
