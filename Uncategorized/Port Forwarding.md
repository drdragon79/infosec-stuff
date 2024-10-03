# SSH
### Local Port Forward
- `SSH Client` listens for a connection, on a specific port
- When `SSH Client` recieves a connection, it tunnels the connection to the `SSH Server`  which connects to the configured destination port.
- Can be used to connect to Internal resource from the outside.
```bash
ssh -L [Client_IP]:<Client_Port>:<Remote_IP>:<Remote_Port> <server/IP>
# By default Client_IP binds to localhost
```
Example:
```bash
ssh -L 0.0.0.0:90:192.168.10.12:80 192.168.10.87
```
- Opens up port on `0.0.0.0` on port `90` on `SSH Client` which when accessed forwards the connection to `192.168.10.12` on port `80` which is only accessible via `192.168.10.87`
### Remote Port Forward
- `SSH Server` listens for a connection on a configured port.
- When the port receives the connection it forwards the connection to the `SSH Client` machine on the configured destination port
- Can be used to expose Client localhost to the public if `SSH Server` is available on public internet
```bash
ssh -R [Server_IP]:<Server_Port>:<Client_IP><Client_Port> <server/IP>
```
Example:
```bash
ssh -R 0.0.0.0:8080:localhost:80 myserver.com
```
- Opens a port `8080` on `myserver.com` which forwards the incoming connection the `SSH Client`'s localhost on port `80`
# Socat
- Useful when SSH is not available.
- Does not come preinstalled. Need to transfer socat binary to host. 
```bash
socat TCP4-LISTEN:1234,fork TCP4:1.1.1.1:4321
```
- Opens up port `1234` on the host machine which forwards all traffic to `1.1.1.1` on port `4321`.
- Similar to local port forwarding. Can be used to access remote servers.
# SOCKS Proxy
- Can port forward all port dynamically
### SSH
```bash
ssh -D [Local_ip]:<Local_Port> myserver.com
# Opens port on SSH Client machine
ssh -R <Remote_Port> myserver.com
# Opens port on SSH Server machine
```
Example:
```bash
ssh -D 9090 myserver.com
```
- Opens up a dynamic port `9090` on `localhost`. 
- We can use proxy chains to use our tools with this forwarded port.
- We can edit `/etc/proxychains.conf` to configure the port.
```bash
[proxyList]
socks4 127.0.0.1 9090
```
- Now we can execute any program through proxy using `proxychains` command
```bash
proxychains curl http://remotehost.com
# will forward the http request to remotehost.com via socks proxy on localhost port 9090 
```