### Sharphound
Sharphound is the tools used for the enumeration of the Active Directory Network.
* Produced a zip file that can be import to bloodhound.
* Should preferebly be run on a machine that is not domain-joined with `runas.exe` to evade AD on domain joined machine. 
```bash
# Import sharphound.ps1
. .\Bloodhound\collectors\Sharphound.ps1

#Invoke bloodhound
Invoke-Bloodhound --CollectionMethods <methods> --Domain <domain> --ExcludeDCs

# Stealth option for less noise
Invoke-Bloodhound -Steath
```
or
```powershell
# Sharphound Executable
.\sharphound.exe

# Stealth option
.\sharphound.exe --steath
```

* `CollectionMethods`: Determines what type of data sharphound collects, `All`
* `Domain` : Domain that is being enumerated
* `ExcludeDCs`: Tells sharphound that exclude Domain Controllers from the enumeration to prevent alerts.
#### Available Sharphound collectors
* `Sharphound.ps1`: Powershell script for enumeration. Can be loaded directly into memory and evades on-disk AV scans.
* `Sharphound.exe`: Binary executable format.
* `Azurehound.ps1`: Sharphound version for azure. Used to enumerate attack path in azure Identity and access management.
### Bloodhound
The GUI that allows us to import and vizualise the data captured by sharphound.
```bash
./bloodhound --no-sandbox
```
Data provided by bloodhound:
- **Overview** - Provides summaries information such as the number of active sessions the account has and if it can reach high-value targets.  
- **Node Properties** - Shows information regarding the AD account, such as the display name and the title.  
- **Extra Properties** - Provides more detailed AD information such as the distinguished name and when the account was created.  
- **Group Membership** - Shows information regarding the groups that the account is a member of.  
- **Local Admin Rights** - Provides information on domain-joined hosts where the account has administrative privileges.  
- **Execution Rights** - Provides information on special privileges such as the ability to RDP into a machine.  
- **Outbound Control Rights** - Shows information regarding AD objects where this account has permissions to modify their attributes.  
- **Inbound Control Rights** -  Provides information regarding AD objects that can modify the attributes of this account.
#### Session Data
The structure of AD does not change very often in large organisations. There may be a couple of new employees, but the overall structure of OUs, Groups, Users, and permission will remain the same.

However, the one thing that does change constantly is active sessions and LogOn events. Since Sharphound creates a point-in-time snapshot of the AD structure, active session data is not always accurate since some users may have already logged off their sessions or new users may have established new sessions. This is an essential thing to note and is why we would want to execute Sharphound at regular intervals.

A good approach is to execute Sharphound with the "All" collection method at the start of your assessment and then execute Sharphound at least twice a day using the "Session" collection method. This will provide you with new session data and ensure that these runs are faster since they do not enumerate the entire AD structure again. The best time to execute these session runs is at around 10:00, when users have their first coffee and start to work and again around 14:00, when they get back from their lunch breaks but before they go home.