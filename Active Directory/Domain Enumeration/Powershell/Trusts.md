### ADModule
```powershell
# Get list of Trust in current domain
Get-ADTrust -Filter *
Get-ADTrust -Domain us.dollarcorp.moneycorp.local -Filter *
```
### Powerview
```powershell
# List all domain trust from the current domain
Get-NetDomainTrust
Get-NetDomainTrust -Domain us.dollarcorp.moneycorp.local
```