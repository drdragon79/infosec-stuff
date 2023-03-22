### LocalAccountTokenFilterPolicy
- Feature implimented by [[UAC]] that strips any local account of its administrative privileges when loggin in remotely.
- Can be disabled using [[Commands#reg]]:
```powershell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
``` 