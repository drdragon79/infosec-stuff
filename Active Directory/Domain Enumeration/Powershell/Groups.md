### ADModule
```powershell
# Get group information
Get-ADGroup

# Get all property of all groups
Get-ADGroup -Filter * -Properties *

# Get group that contains admin
Get-ADGroup -Filter 'Name -like "*admin*"' | Select-Object Name
Get-ADGroup -Filter * -Properties * | Where-Object {$_.Name -Like "*admin*"} | Select-Object -Property Name

# Get members of a group "Domain Admins"
Get-ADGroupMember -Identity "Domain Admins"

# NOTE: We can groups inside a group hence IsGroup property is true for group and false for a user. 
# User -Recursive to enumerate all the users nested inside a group 
Get-ADGroupMember -Identity "Administrator" -Recursive


# Get group membership of a user "Student"
Get-ADPricipalGroupMembership -Identity "Student"

```

### Powerview
```powershell
Get-DomainGroup
Get-DomainGroup -Domain anotherdomain.local
Get-DomainGroup -FullData

# search group containing *admin*
Get-DomainGroup *admin*

# Get members of the group 
Get-DomainGroupMember -GroupName "Domain Admins" -Recurse

# Get Group membership of a user
Get-DomainGroup -Username "student1"

# List local groups on a machine
# Other than the domain controller we need to administrative privs on the computer to get local groups 
Get-NetLocalGroup -ComputerName dc.domain.local
# Get members of the local group
	Get-NetLocalGroupMember -ComptuerName dc.domain.local -GroupName Administrator
```