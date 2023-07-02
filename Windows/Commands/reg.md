### query 
```powershell
reg query <key> [options]
```
- `/v <value>` : Specify value to be queried. If not supplied, all values are printed
- `/t <type>` : Specifiy registriy type to be queried
- `/s` : Query recursively
- `/f <data>`: specify data to search for 
### add
```powershell
reg add <key> [options]
```
- `/v <value>` : value to add or modify
- `/t <type>` : type of the value
- `/d <data>` : data to add
- `/f` : do not ask for confirmation
### save
saves a copy of a key, entry and values
```powershell
reg save <key> <filename> [options]
```
- `/y` : overwrite the files

