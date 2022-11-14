### Registry
Can can contain password for unsecure services.
[[Commands#reg]]
```powershell
# search for password in either keyname or keyvalue recursively
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
# 
```

### Saved Creds
- Windows can allow user to save their password with runas.exe command.
- Can be queried with:
```powershell
# list already saved key
cmkey /list

```
