### System
```powershell
# Get Systeminfo
systeminfo
systeminfo | findstr /B /C:"Os Version"

# Get hotfix
wmic qfe # quick fix engineering

# Get list of drives and filter
wmic logicaldisk get caption,discription,pridername
```

### User Enumeration
```powershell
# print current user
whoami

#list privilege of current user
whoami /priv

# List user of the current group
whoami /groups

# print users on the computer and get details
net user
net user <username>

# list local groups
net localgroup
net localgroup administrator
```

### Network Enumeration
```powershell
# List ip addresses, interface etc
ipconfig 
ipconfig /all # verbose

# get ARP table
arp -a

# socket enumration
netstat
```

### Password Hunting
```powershell
# search for "password" in *.txt files
findstr /si Password *.txt
```

### AV Enumeration
```powershell
# Check if windows defender is running
sc query windefend

#list services
sc queryex service

# Firewall Enumeration
netsh advfirewall firewall dump
netsh firewall show state
```

# Automated Tools
### Executables
- winPEAS.exe
- Sealbelt.exe
- Watson.exe
- SharpUp.exe
### Powershell
- Sharlock.ps1
- PowerUp.ps1
- jaws-enum.ps1
### Other
- Windows-exploit-suggester (local)
- Exploit Suggester (metasploit)