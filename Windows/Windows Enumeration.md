# Command Line
1. System
```powershell
# Get Systeminfo
systeminfo
systeminfo | findstr /B /C:"Os Version"
systeminfo | findtr Domain

# Get hotfix
wmic qfe # quick fix engineering

# Get list of drives and filter
wmic logicaldisk get caption,discription,pridername
```
2. User Enumeration
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
3. Network Enumeration
```powershell
# List ip addresses, interface etc
ipconfig 
ipconfig /all # verbose

# get ARP table
arp -a

# socket enumration
netstat -abno
```
4. Password Hunting
```powershell
# search for "password" in *.txt files
findstr /si Password *.txt
```
5. AV Enumeration
```powershell
# Check if windows defender is running
sc query windefend

#list services
sc queryex service

# Firewall Enumeration
netsh advfirewall firewall dump
netsh firewall show state
```

# WMI
1. Antivirus Details
```powershell
# Installed Antivirus solutions
Get-WmiObject -Namespace root\securitycenter2 -Class AntiVirusProduct

# Defender status
Get-MpComputerStatus
```
2. Services
```powershell
Get-WmiObject -Namespace root\cimv2 -Class Win32_service
```
3. Processor Architecture
```powershell
Get-WmiObject -Namespace root\cimv2 -Class win32_processor
```
4. Logged On User
```powershell
Get-WmiObject -Class win32_ComputerSystem | Select-Object -Property Username
```
5. Installed HotFix
```powershell
Get-WmiObject -Class Win32_quickfixengineering
```
6. Get log files locations
```powershell
Get-WmiObject -Class win32_NTeventlogfile
```
7. Get Command Line to start process
```powershell
Get-WmiObject -Class win32_process | Select-Object -Property ProcessName,CommandLine
```
8. Get BinPath for running services
```powershell
Get-WmiObject -Class win32_service | select DisplayName,Pathname
```
9. Routing table
```powershell
Get-WmiObject -Class Win32_IP4RouteTable
```
10. User Accounts
```powershell
Get-WmiObject -Class Win32_UserAccount
```
11. Groups
```powershell
Get-WmiObject -Class Win32_Group
```
12. Shadow Copy Information
```powershell
Get-WmiObject -Class Win32_ShadowCopy
# Shadown copy class can be used to call the Create method to create shadow copy
# Create a shadow copy and creating a link to that shadow copy.
(Get-WmiObject -Class Win32_ShadowCopy -List).Create("C:\", "ClientAccessible")
$link = (Get-WmiObject -Class Win32_ShadowCopy).DeviceObject + "\"
cmd /c mklink /d C:\shadowcopy "$link"
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