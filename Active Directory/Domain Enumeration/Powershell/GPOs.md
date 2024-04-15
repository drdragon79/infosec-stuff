GPOs are used to manage configuration and changes centrally in AD
Allows configuration of:
* Security Settings
* Registry Based policy settings
* Group policy preference like startup/shutdow/logon scripts
* Software Installation

GPOs are abused for attacks like:
* Privilege Escalation
* Backdoors
* Persistence
### ADModule
```powershell
# Get list of GPOs in the current domain
Get-GPO -All # from group policy module
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\Users\Admin\report.html
```
### Powerview
```powershell
# Get list of group policy in current domain
Get-DomainGPO

# List which GPOs are applied to student.domain.local
Get-DomainGPO -ComputerIdentity student.domain.local

# Get GPOs that use restricted groups or groups.xml for interesting users
Get-DomainGPOLocalGroup

## NOTE: There is no powerview function to get resultant set of policy 

# Get users which are in a local group of a machine using GPO
Get-DomainGPOComputerLocalGroupMapping -ComputerIdentity computer1

# Get machines where the given user is a member of a specific group
Get-DomainGPOUserLocalGroupMappting -Identity username -Verbose

# Get users which are a part of a local group of a machine using GPO
Find-GPOComputerAdmin -CommputerName comp.domain.local

# Get machines where the given user is a member of a specific group
Find-GPOLocation -Username student1 -Verbose
```
