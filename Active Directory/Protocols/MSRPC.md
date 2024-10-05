- Inter Process Communication (IPC)
- Windows exposes a lot of common interface for other machines to connect to.
- MS-RPC uses the following mechanism to expose these Remote Procedure Calls:
	- TCP
	- SMB
### TCP
- PORT 135 is the endpoint mapper
- Maps all the RPC endpoints to the TCP ports (49152 to 65535) available on that computer
### HTTPS
- PORT 593 is the endpoint mapper 
### SMB
- PORT 445
- [SMB](SMB.md) using its [IPC$](SMB#IPC$) share, exposes named pipes which can be used to perform RPC calls.
### Ephemeral Range
- Range of TCP ports directly hosting RPC application

# Enumeration
### Impacket's rpcdump.py
```bash
python rpcdump.py <target>
```
##### Bindings
- `ncalrpc`: Local IPC.
- `ncacn_np`: IPC running over SMB as named pipes.
- `ncacn_ip_tcp`: IPC running over TCP protocol.

### rpcclient
```bash
# Connect
		rpcclient -U <username>%<password> <target>

# Test null session
rpcclient -U "" -N <target>
```
##### rpcclient commands
```bash
# Domain information query
querydominfo

# Domain Lookup (Prints domain name and SID)
lookupdomain <domainname>

# Enumerate domains
enumdoms

# List domain users
enumdomusers

# List domain groups
enumdomgroups

# Create groups
createdomgroup <groupname>

# Delete group
deletedomgroup <groupname>

# Print the groups that the user is a memeber of
queryusersgroups <userRID>

# Print the members of the Group
querygroupmem <groupRID>

# List details about a group
querygroup <groupRID>

# List user details
queryuser <username>

# List prvileges
enumprivs

# Print password policy
getdompwinfo

# Print password policy for specific user
getusrdompwinfo <userRID>

# List SIDs of user
lsaenumsid

# Creating domain users
createdomuser <username>
setuserinfo2 <username> 24 <password>

# Lookup users (prints username along with SID)
lookupnames <username>

# Delete Domain users
deletedomuser <username>

# List shares
netshareenum
netshareenumall

# Print details of the specific share
netsharegetinfo <sharename>

# Change password for a user
chgpasswrd raj <oldpass> <newpass>
```