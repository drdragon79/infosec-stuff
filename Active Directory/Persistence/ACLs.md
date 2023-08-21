- With DA privs, the ACL of the domain root can be modified to provide usefull rights  like FullControl or the ability to run DCSync.
```powershell
# Give yourself Fullcontrol over the root domain object
Add-ObjectAcl -TargetDisguishedName 'DC=mydomain,DC=local' -PrincipalSamAccountName doctordragon -Rights All -Verbose
# Give yourself DCSync rights over the root domain object
Add-ObjectAcl -TargetDisguishedName 'DC=mydomain,DC=local' -PrincipalSamAccountName doctordragon -GUIDRight DCSync -Verbose
```
# AdminSDHolder
- A control that holds a blueprint of A  CLs for certain built-in privileged group called Protected Groups
- A Security Descriptor Propagator (SDPROP) runs every hour and overwrites the ACL from this container to all the member of the protected group.
### Protected Groups
- Account Operators
- Backup Operators
- Server Operators
- Print Operators
- Domain Admins
- Replicator
- Enterprise Admins
- Domain Controllers
- Read-only Domain Controllers
- Schema Admins
- Administrators
#### Well Known Groups for Abuse
- Account Operator - Cannot modify DA/EA/BA groups. Can modify nested groups within these groups
- Backup Operator - Backup GPO, edit or add SID of controlled account to a privileged group and restore
- Server Operators - Run a command as SYSTEM (using disabled browser service)
- Print Operator - copy ntds.dit backup, load device drivers
### AdminSDHolder Abuse
- If we can add our user to the AdminSDHolder object and give ourselves full control, the ACLs will propagate to all the protected objects and we will have full control over the protected objects.
```powershell
# Powerview module to add ACL to the AdminSDHolder group
Add-ObjectAcl -TargetADSPrefix 'CN=AdminSDHolder,CN=System' -PrincipalSamAccountname student1 -Rights All -Verbose
# We can change all to be ResetPassword,WriteMembers
# Run SDPropagator
Invoke-SDPrpagator -timeoutMinutes 1 -showProgrss -Verbose
```
# Security Descriptors
- The SDDL (Security Descriptor Definition Language) defines the format used to descibe a security descriptor. SDDL uses ACE string for SACL and DACL.
```powershell
ace_type;ace_flag;rights;object_guid;inherit_object_guid;accountSID
```
- We can edit the security descriptor or the remote WMI and DCOM Object to provide ourselves full control over those object which can let us run WMI and DCOM remotely. This can be done by giving ourselves rights to connect to DCOM and rights to the namespace.
1. DCOM: Component Serivces -> My Computer ->Properties ->  COM Security -> Edit Links -> Add User
2. WMI: Computer Management -> WMI Control -> Properties -> Security -> Add User