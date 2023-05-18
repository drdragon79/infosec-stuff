```powershell
$env.USERDOMAIN
```
### .NET
```powershell
$AD = [System.DirectoryServices.ActiveDirectory.Domain]
$AD::GetCurrentDomain()
```

### ADModule
```powershell
#Get information about current domain
Get-ADDomain
(Get-ADDomain).DNSRoot

#Get information about a different domain if trust is configured
Get-ADDomain -Identity differentdomain.local

#Get SID of a domain
(Get-ADDomain).DomainSID

# Get Domain Controller of current Domain
Get-ADDomainController

# Get Domain controller from different Domain
Get-ADDomainController -DomainName mydomain.local -Discover
```

### Powerview
```powershell
# List domain information
Get-NetDomain
Get-NetDomain -Domain anotherdomain.local

# Get SID of the Domain
GetDomainSID

# Get Domain Policy of the Domain
Get-DomainPolicy
(Get-DomainPolicy)."system access"
Get-DomainPolicy -Domain anotherdomain.local
(Get-DomainPolicy -Domain anotherdomain.local)."system access"

# Get Domain Controller for the domain
Get-NetDomainController
Get-NetDomainController -Domain anotherdomain.local
```

### WMI
```powershell
(Get-WmiObject Win32_ComputerSystem).Domain
```