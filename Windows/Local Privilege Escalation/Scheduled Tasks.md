- Windows can run tasks at certain time or at certain action.
- Tasks run with the user privilege of the user who created them.
- Tasks can be configured to be run with SYSTEM privilege
### Enumerate Tasks
- cmd
```powershell
schtasks /query /fo LIST /v
```
- Powershell
```powershell
Get-ScheduledTask | where-object {$_.TaskPath -notlike "\Microsoft"} | Format-Table TaskName,TaskPath,State
```
- If a scipt is running as SYSTEM or admin, and we have write permission on the file, we can change the content of the files with our revere shell.