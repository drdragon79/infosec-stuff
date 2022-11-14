# accesschk.exe
Tool to check access that a <user,group> has to a <file,directory,regkeys,objects,service>.
```powershell
# should be run first to accept the eula
.\accesschk /accepeula 

#syntax
.\accesschk <options> [username] <service|files|directory|regkeys>
# username is optional. Prints access for every username 
# by default, the object is a directory
```
### Options
- `-q`: omit banner
- `-u`: suppress errors
- `-v`: verbose
- `-w`: prints write access only
- `-r`: prints read access only

### Check ACL of users on a service
```powershell
# prints access of the users/user on a service
.\accesschk -quvc [username] <service>
```
- `-c`: service name is supplied

### Check ACL on a directory
```powershell
# Prints access of the all users/user on a directory
.\accesschk -quvd [username] <directory>
# add w to check only for write access to a directory
```
- `-d`: only print access for top level directory and not recursive

### Check ACL on registry keys
```powershell
.\accesschk -quvk <registry key>
```
- `-k`: registry key is supplied

