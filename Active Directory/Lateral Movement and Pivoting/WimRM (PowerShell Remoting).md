#powershell #winrs 
- Ports: 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- Group Required: Remote Management Users
- PsExec on steroids
- If Admin creds are used, we get a elevated shell on the remote machine (No UAC issues), because the remoting process runs a a High Integrity Process.
- PSRemoting uses WinRM which is MS's implementation of WS-Management
- Enabled by default since Server 2012
- Need to manually enable in Windows Desktop (Requires Adminitrative Privs)
- Some disadvantages:
	- Supports systemwide transcript and deep script logging
### One-to-One
- Interactive login to one machine
- Runs in a new process (wsmprovhost)
- State-full (persistent variables and state) using `New-PSSession`
- Commands:
	- `Enter-PSSession` : Enter interactive prompt on the target machine
	- `New-PSSession` : Returns sessions to create persistent environment.
```powershell
# Create interactive session using the current credentials
Enter-PSSession -ComputerName mymachine.domain.local

# Create session
$sess = New-PSSession -ComputerName mymachine.domain.local
$sess # holds the credentails and ohter information

# Enter-PSSession can be used to connnec to this session
Enter-PSSession -Session $sess

# Use Credentails to connect to the remote machine
# Creat PScredentails object that holds username and password
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
# Connect to the machine
Enter-PSSession -Computername TARGET -Credential $credential
```

### One-to-Many
- Also knows as fan-out remoting.
- Non Interactive
- Executes commands parallely.
- Commands:
	- `Invoke-Command` : Executes command on one more machine parallely.
- Run commands/scripts on:
	- one more more computers
	- disconnected session
	- as a background job
- Required administrative access on the target machine.
```powershell
# Execute Command
Invoke-Command -ComputerName mycomputer.mydomain.local -ScriptBlock {whoami}

# Execute a fiile
Invoke-Command -ComputerName mycomputer.mydomain.local -FilePath .\Temp.ps1

# Can also run local functions on the target machine
Invoke-Command -ComputerName mycomputer.mydomain.local -ScriptBlock ${function:mylocalfunction}

# Can also use already created session from New-PSSession
Invoke-Command -FilePath C:\script.ps1 -Session $session

# Specify Multiple Servers
Invoke-Command  -Scriptblock {whoami} -Computername (Get-Content servers.txt)
```
- We can also get host process information for the running PS Remoting session
```powershell
Get-PSHostProcessInfo
```
### winrs
- Evades powershell based logging
- Uses WinRM ports - 5985 and 5986
```cmd
winrs.exe -u:<username> -p:<password> -r:<server> cmd
```