- User can define apps that can startup when login by placing shortcuts in specific directory.
- Apps are stored in a startup directory for all users:
```powershell
"C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"
```
- If we have permission to create files in this directory, we can use reverse shell to get a shell when the admins logs in. 