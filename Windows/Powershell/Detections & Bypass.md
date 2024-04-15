- System wide transcription
- Script block logging
- AMSI
- Constrained language mode (CLM) - Integrated with AppLocker and WDAC

### Execution Policy
- Not a security control.
- Prevents the user from accidentally running scripts
- To disable: 
```powershell
powershell -ExecutionPolicy bypass
powershell -c "command"
$env:PSExecutionPolicyPreference = "bypass"
```

# Detections
### AMSI
We can use [AMSITrigger](https://github.com/RythmStick/AMSITrigger) and [DefenderCheck](https://github.com/matterpreter/DefenderCheck) to check our powershell script and binaries for detection.
```powershell
.\AMSITrigger_x64.exe -i C:\Tool\BadPowershellCode.ps1
.\DefenderCheck.exe PowerUp.ps1
```
# Bypass
## Loggining & Monitoring Bypass
#### Invisi-Shell
- [Github](https://github.com/OmerYa/Invisi-Shell)
- Disables
	- System Wide Transciption
	- Script Block Logginig
	- ASMI
## AMSI Bypass - Ofuscator
#### Invoke-Obfuscation
- [Github](https://github.com/danielbohannon/Invoke-Obfuscation)