### ADModule
```powershell
# Get List of OU in the domain
Get-ADOrganizationalUnit -Filter * Properties *

# Get GPO applied on an OU
# gplink attribute of an OU gives GPO ID of the GPO applied to the OU
Get-GPO  -Guid <GPO_ID>
```
### Powerview
```powershell
# Get OUs in a Domain
Get-NetOU -FullData

# Get GPO applied on an OU
# gplink attribute of an OU gives GPO ID of the GPO applied to the OU
Get-DomainGPO -Identity "{GPO_ID}"
```