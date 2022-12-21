SERVER MESSAGE BLOCK
## smbmap.py
```bash
smbmap.py -H <IP> [options]
```
- `-u <username>`
- `-p <password>
- `-r <sharename>` : list content of the share
- `-x <command>` : execute command remotely
- `--upload <file> <sharepath>` : upload to file share
- `--down <file>` : download from file share