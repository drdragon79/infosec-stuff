#schtasks 
# schtasks.exe
- Remotely schedule tasks.
```powershell
# Create a task
schtasks.exe /s <server> /ru "SYSTEM" /create /tn "mytask" /tr "command/payload" /sc ONCE /sd 01/01/1970 /st 00:00

# Run the task
schtasks.exe /s <server> /run /tn "mytask"

# Delete the task
schtasks.exe /s <server> /tn "mytask" /delete /f
```