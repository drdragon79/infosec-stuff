- A control that holds a blueprint of ACLs for certain built-in privileged group called Protected Groups
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
- If we can add our user