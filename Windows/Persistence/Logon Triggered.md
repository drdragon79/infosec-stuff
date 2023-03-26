### Startup Folder
- User Startup Folder - Runs all programs in the directory of the `<username>` logs in
```powershell
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
- Global Startup Folder - Force all user to run programs in this directory
```powershell
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
```

### Registry - Run/RunOnce
- Registry has the following location for application running at logon
	- `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` - Current User - Run at logon
	- `HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce` - Current User - Run only once
	- `HKLM\Software\Microsoft\Windows\CurrentVersion\Run` - All Users - Run at logon
	- `HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce` - All Users - Run only once
- Create key value pair:
```powershell
backdoor -> C:\Windows\reverse_shell.exe
```
### Winlogon
### Logon Scripts
