### System Information
```bash
# OS Version
cat /etc/*release
cat /etc/os-release # typicall, arch
cat /etc/lsb-release # Debian
cat /etc/fedora-release # fedora

# Hostname
hostname

# Users & Groups
cat /etc/passwd
cat /etc/gr

# Kernel
uname -a
```

### Packages
```bash
ls /usr/bin
ls /sbin

# Debian
dpkg -l
apt list --installed

# Arch
pacman -Q

#Fedora
rpm -qa
```

### Users
```bash
# list users
cat /etc/passwd

# logged in user
who

# logged in user and what they are doing
w

# list last logged on users
last
```

### Networks
```bash
# list network addapters and ip addresses
ip address # or
ip a

# DNS server
cat /etc/resolv.conf

# socket details
ss -pant
netstat -plt
netstat -atupn

# files open
sudo lsof # list all open files
sudo lsof -i # list open files via network

```
### Process
```bash
# List running process
ps aux

# ps but tree output (f flag)
ps auxfj

```