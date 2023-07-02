[Microsoft's Documentation](https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)

### SeBackup/SeRestore
- Allows users to read and write to any file or directory on the system, ignoring any permission of ACLs in place.
- Purpose of this privilege is to allow user to create a backup of the system without Admin of SYSTEM privs.
- Can be abused by creating a backup of the SYSTEM and SAM registry hive.
### SeTakeOwnership
- This privilege can be use to take ownership of any object(files, registry keys) in the system.
- Can take ownership of an executables that runs with SYSTEM privs and get a system shell.
