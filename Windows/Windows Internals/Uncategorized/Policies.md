### LocalAccountTokenFilterPolicy
- Feature implemented by [User Account Control](../Security/User%20Account%20Control.md) that strips any local account of its administrative privileges when login in remotely.
- Can be disabled using [reg](../../Commands/reg.md#add):
```powershell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
``` 