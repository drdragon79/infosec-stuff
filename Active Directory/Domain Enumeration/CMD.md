We can use the `net` command to enumerate users and computer of the domain. 
### NET Command
```bash
# list the users of the commadn 
net user /domain

# get details about a user
net user doctor.dragon /domain

# list groups present in the domain
net group /domain

# Get details about a group
net group "Tier 1 admins" /domain

# to get password policy of the domain
net accounts /domain
```

* This command cannot be used on non-domain joined computer. 
NOTE:
- Powershell ADModule uses ldap for querying the result. But `net user /domain` uses SAMR protocola for the querying the same information.