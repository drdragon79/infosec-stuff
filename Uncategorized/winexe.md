winexe uses the admin$ and rpc (`net service start`) to install and start a remote service (winexesvc). This service initialize a _Named Pipe_ that is used to transport commands from the client to the server and the output in the reverse way. When finish, the _named pipe_ is closes and the winexesvc waits or more connections or is uninstalls (depending of the options given)

The access to the admin$ share and rpc uses the normal SMB protocol (SMB1 and SMB2 in more recent versions due windows 8.1 not supporting anymore SMB1)

This is also the way that psexec work (but it uses SMB2 with fallback to SMB1 for several years)