- Used to harvest credentials from a windows machine.
- Has two additional components:
	- `Mimidrv`: driver that interacts with windows kernel
	- `Mimilib`: Bypass applocker, auth package/SSP etc.
- Runs with:
	- `Administrator` with `Privilege::debug` mode
	- `SYSTEM` privilege

# Kerberos
### ask
```bash
kerberos::ask
```
### list
- Lists or exports TGTs and STs from the current session
```powershell
kerberos::list # Lists the tickets
kerberos::list /export # Export the tickets to files
```
### purge
- purge all TGTs and STs from the current session.
```powershell
kerberos::purge
```
### tgt
- Prints information about the TGT from current session
```powershell
kerberos::tgt
```
### golden
- Create golden tickets from krbtgt's NTLM hash or Kerberos keys.
### silver
- Create silver ticket from a service's NTLM hash or Kerberos keys.\

# SekurLSA
# LSAdump