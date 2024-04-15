### ADModule
```powershell
# Get list of all computers
Get-ADComputer

# Filter computers
Get-ADComputer -Filter * | select Name

# Get all Properties of all computer
Get-ADComputer -Filter * -Properties *

# Filter based on Operating system
Get-ADComputer -Filter * | Where-Object {$_.OperatingSystem -like "*Server 2016*"} | Select-Object -Property Name, OperatingSystem

Get-ADComputer -Filter 'OperatingSystem -like "*server 2016*"' -Properties OperatingSystem | Select-Object Name,OperatingSystem

# Check for live machines
Get-ADComputer -Filter * -Properties DNSHostName | %{Test-Connection -Count 1 -ComputerName $_.DNSHostName}

```

### Powerview
```powershell
# Get list of all the computer ojbect
Get-DomainComputer | Select-Ojbect Name

# Query computers based on operating system
Get-DomainComputer -OperatingSytem "*Server 2022*"

# Test connectin to a computer
Get-DomainComputer -Ping
```
