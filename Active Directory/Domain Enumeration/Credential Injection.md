> _"If you know the enemy and know yourself, you need not fear the results of a hundred battles. If you know yourself but not the enemy, for every victory gained you will also suffer defeat."_ - Sun Tzu, Art of War.

## runas.exe
* A command line tool to inject credentials into memory
* Usefull when we have a valid domain credentials but no access to a domain joined machine.
* As we are not domain joined, we need to specify the domain in the management console or powershell 
```cmd
runas /netonly /user:<domain>\<username> cmd.exe
```
* `/netonly`: Loads crendetials for network authentication. Commands are executed locally but network authentication will be performed over the domain account context.
* `/user:<domain>\<username>`: Username of the domain account we want to authenticate. It is recommended to use Fully quallyfied domain name instead of netbions name
* `cmd.exe`: Command to execute.
- The logon type used by `runas` is logon type 9, which is different from the normal logon. Only programs that requires remote authentication, will use the new injected credentials. Eg., using `dir` on a remote share, will use the injected credentials.

#### Credential Verification
The credentials are not verfied when we pass it to `runas`. To Verify the credentials, we cal try to list the `SYSVOL` directory.

The `SYSVOL` directory is a special directory that is available to all the domain users. It is required to push the GPOs to the Users or Computers.

```powershell
PS C:\Windows\system32> dir \\za.tryhackme.com\SYSVOL

    Directory: \\za.tryhackme.com\SYSVOL             
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----l         2/24/2022   1:57 PM                za.tryhackme.com
```

### IP vs HOSTNAME
When we provide the hostname, network authentication will attempt first to perform Kerberos authentication. Since Kerberos authentication uses hostnames embedded in the tickets, if we provide the IP instead, we can force the authentication type to be NTLM. 

In some instances, organisations will be monitoring for OverPass- and Pass-The-Hash Attacks. Forcing NTLM authentication is a good trick to have in the book to avoid detection in these cases.

### Change DNS of Non-Domain machine via Powershell
```powershell
$dnsip = "<DomainControllerIP>"
$index = Get-NetAdapter -Name '<Interface>' | Select-Object -ExpandProperty 'ifIndex'
Set-DnsClientServerAddress -InterfaceIndex $index -ServerAddresses $dnsip
```