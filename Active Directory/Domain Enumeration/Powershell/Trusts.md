### CMD
```cmd
nltest /domain-trust
```
### ADModule
```powershell
# Get list of Trust in current domain
Get-ADTrust -Filter *
Get-ADTrust -Domain us.dollarcorp.moneycorp.local -Filter *
```
### Powerview
```powershell
# List all domain trust from the current domain
Get-DomainTrust
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local
```