- Most of the enumeration mixes really well with the normal traffic to the DC.
- Hardening can be done on the DC (or other machines) to contain the information provided by the queried machine.
### NetCease
- NetCease is a script which changes permission on the NetSessionEnum method by removing permission for Authenticated User group.
- This fails many attacker's session enumeration and hence user hunting capabilities.
```powershell
.\NetCease.ps1
```

Another Interesting script from the same author is SAMRi10 which hardens windows 10 and Server 2016 against enumeration which uses SAMR Protocol