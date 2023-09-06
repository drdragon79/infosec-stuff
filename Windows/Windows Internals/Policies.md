### LocalAccountTokenFilterPolicy
- Feature implimented by [[User Account Control]] that strips any local account of its administrative privileges when loggin in remotely.
- Can be disabled using [[reg#reg]]:
```powershell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
``` 