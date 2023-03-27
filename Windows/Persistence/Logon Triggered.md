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
- Winlogon - windows component that loads user profile after user authenticates
- Winlogon registry keys are located at : `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon`
- Interesting Keys in the registry:
	- `Userinit` -> userinit.exe (restores the user preference)
	- `Shell` -> explorer.exe (system's shell)
- We can append our reverse shell in userinit.exe after `,`
- `Userinit` -> userinit.exe,reverse_shell.exe
### Logon Scripts
- Apart from loading the user profile, userinit.exe checks for an environment variable `UserInitMprLogonScript`. This environment variable can be used to run scripts when user logs in.
- This variable is located at: `HKCU\Environment`
- `UserInitMprLogonScript` -> `C:\Windows\reverse.exe`
