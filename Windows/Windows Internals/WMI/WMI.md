- Windows Management Instrumentation
- Microsoft's Implimentation of:
	- CIM: Common Information Model
	- WBEM: Web Based Enterprise Management
- Provides a uniform interface for applications/script to manage a local or remote cmputer or network
# Architecture
![[Pasted image 20221216201142.png]]
# Components
### MOF Files (managed object format)
- Defines WMI namespace, classes providers etc.
- Stored at `%WINDIR%\system32\wbem` with extention of `.mof`
### Providers
 - associated with every MOF file (not true for all `.mof` files)
 - eg. `filename.mof and filename.dll`
 - providers works as a bridge between a managed object and WMI. A providers main function is to provide access to classes.
### Managed Object
- Component that is being managed by WMI, could be process, service, operating system.
### Namespaces
- Created by providers.
- Used to divide classes logically
- Groups - 
	- System
	- Core
	- Extension
- Types - 
	- Abstract
	- Static
	- Dynamic
- Makes classes easy to discover and use
- Well Known Namespaces - 
	- `root\cimv2`
	- `root\default`
	- `root\security`
	- `root\subscription`
### Repository
- Database that is used to store definition of classes
- `%WINDIR\System32\wbem\Repository`
### Consumers
- Application of script that can be used to interact with WMI classes for query of data or run methods or to subscribe to events.
- Eg: Powershell,wmic.exe
# Interacting with Powershell
- WMI cmdlets
```powershell
PS C:\> Get-Command *wmi* -Type cmdlet | Select-Object -Property CommandType,Name

CommandType Name
----------- ----
     Cmdlet Get-WmiObject
     Cmdlet Invoke-WmiMethod
     Cmdlet Register-WmiEvent
     Cmdlet Remove-WmiObject
     Cmdlet Set-WmiInstance
```
- Up until Powershell version v2 we had only wmi commands
- From Powershell version v3, we have access to CIM cmdlets
### CIM
- Command Information Model
- Uses WS-MAN and CIM standards to manage objects.
- Can be used if WMI is blocked but WSMAN (WinRM) is enabled. 
- CIM is aligned to the standards of DMTF (Distributed Management Task Force) which means it can work with non-windows box as well.
NOTE: WMI is Windows oriented version of CIM
```powershell
PS C:\Users\akshay> Get-Command *cim* -Type cmdlet | Select-Object -Property CommandType,Name

CommandType Name
----------- ----
     Cmdlet Get-CimAssociatedInstance
     Cmdlet Get-CimClass
     Cmdlet Get-CimInstance
     Cmdlet Get-CimSession
     Cmdlet Invoke-CimMethod
     Cmdlet New-CimInstance
     Cmdlet New-CimSession
     Cmdlet New-CimSessionOption
     Cmdlet Register-CimIndicationEvent
     Cmdlet Remove-CimInstance
     Cmdlet Remove-CimSession
     Cmdlet Set-CimInstance
```
# Namespace
- List available namespace by querying the `__Namespace` class
```powershell
# WMI
Get-WmiObject -Namespace root -Class __Namespace

# CIM
Get-CimInstance -Namespace root -ClassName __Namespace
```
- Every Namespace has class called `__Namespace` that can be used to query classes in that particular namespace.
- by default `root\CIMv2` is used to query class.
```powershell
# If we query a class without specifying namespace, "root\CIMv2 is used"
Get-WmiObject -Class Win32_Process
```
```powershell
# Recursive function to list all the Namespaces in the system.
function Get-WmiNamespace {
	param (
		$Namespace = 'root'
	)
	Get-WmiObject -Namespace $Namespace -Class __Namespace | forEach-Object {
		($ns = '{0}\{1}' -f $_.__Namespace,$_.Name)
		Get-WmiNamespace $ns
	}
}
```
# Classes
- Classes represents, process, harware, service etc.
### Categories 
- Core Classes: represents managed object that apply to all areas of management. Example - `__SystemSecurity` class
- Common Classes: Extension of core classes. for specific management areas. Prefixed with `CIM_` . Example - `CIM_UnitaryComputerSystem`
- Extended Classes: Technology specific addition to common classes. Example - Win32_ComputerSystem
### Types
- Abstract - Template Class used to define new classes. Cannot be used to retrieve instances.
- Static - Stores data like WMI configuration and operational data.
- Dynamic - Retrieved from a provider and represent a WMI managed object. Things like process, services and System.
- Association - Describes a relationship between two classes or managed resources
### Querying Classes with WMI
- Classes can be listed with `-List` option
```powershell
# List all classes in default "root\CIMv2" namespace
Get-WmiObject -List
Get-WmiObject -Class * -List

# Search class
Get-WmiObject -Class *bios* -List

# Search for specific classes in another namespace
Get-WmiObject -Namespace root\security -Class *bios* -List
```
### Querying Classes with CIM
```powershell
# List all classes in default "root\CIMv2" namespace
Get-CimClass

# Search for a class
Get-CimClass -Class *bios*

# List only dynamic classes, from which instances can be retrieved
Get-CimClass -QualifierName dynamic
```
# Objects
### Querying Objects with WMI
```powershell
# Lists all the instances of the class "Win32_BIOS"
Get-WmiObject -Class Win32_BIOS
```
### Querying Objects with CIM
```powershell
# Lists all the instances of the class "Win32_BIOS"
Get-CimInstance -ClassName Win32_BIOS
```
### Filtering Objects/Instances
1. `-Filter` Parameter
	```powershell
	-Filter "Name = 'explorer.exe'"
	```
2. `Where-Object` cmdlet (slower than `-Filter`)
	```powershell
	Where-Object {$_.name -eq "svchost.exe"}
	#OR
	Where-Object name -eq "svchost.exe"
	```
3. `-Query` Parameter
```powershell
# similar to sql
-Query "select * from Win32_Process where Name = 'lsass.exe'"
```