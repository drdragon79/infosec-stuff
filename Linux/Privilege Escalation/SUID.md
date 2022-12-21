- Binaries with SUID bit set runs with it's owners's privilege.
```bash
# find all files with SUID bit set
find / -type f -perm -04000 -ls 2>/dev/null
```