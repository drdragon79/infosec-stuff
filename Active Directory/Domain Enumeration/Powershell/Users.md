### ADModule
```powershell
# Get all user from antoher domain
Get-ADUser -Server za.tryhackme.com

# Get all properties of all user
Get-ADUser -Filter * -Properties *

# Get some property of a particular username
Get-ADUser -Filter drdragon -Properties Name

# List all the properties that the user has 
Get-ADUser -Filter * -Properties * | Select-Object -First 1 | Get-Member | Select-Object -Property Name

# Search for strings in user's attribute
Get-ADUser -Filter 'Description -like "*built*"' -Properties Description | Select-Object -Property Name,Description
# or
Get-ADUser -Filter * -Property * | Where-Object {$_.Description -Like "*built*"} | Select-Object -Property Name,Description

# Search Users by LDAP hierarchical tree structure
Get-ADUser -Filter * -SearchBase "CN=Users,DC=MYORG,DC=LOCAL"
```
### Powerview
```powershell
# Get all the user
Get-DomainUser

# Get Information about specific user
Get-DomainUser -Idnentity username

# Get property of the user
Get-DomainUser -Identity username -Properties *
Get-DomainUser -Properties *

# Search for strings in user's attribute
Get-DomainUser -LDAPFilter "Description=*build*" | select name,description

# Get actively logged users on a computer 
# Needs local admin rights on the target 
Get-NetLoggedon -ComputerName <servername>

# Get-Locally logged users on a computer 
# Needs remote registry on the target - started by default on a server OS
Get-LoggedonLocal -ComputerName comp.domain.local

# Get the last logged user on a computer (needs admin rights and remote registry on the target)
Get-LastLoggedOn -ComputerName <server>
```
### Other Tools
SessionHunter
```powershell
# Can be used to enumerate remote logged on users on a machine without local admin privileges
Invoke-SessionHunter -FailSafe
# This uses remote registry and uses HKEY_USERS hive
# Opsec verison of the command
Invoke-SessionHunter -NoPortScan -Targets C:\servers.txt
	```