# Default passwords
- [cirt.net](https://cirt.net/passwords)
- [default-password.info](https://default-password.info)
- [datarecover.com](https://datarecovery.com/rd/default-passwords/)
# Wordlist
- [wiki.skullsecurity.com](https://wiki.skullsecurity.org/index.php?title=Passwords)
- [Seclist](https://github.com/danielmiessler/SecLists/tree/master/Passwords)
# Leaked Passwords
- [Seclist](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Leaked-Databases)
# Customized Passwords
- [Cewl](https://github.com/digininja/CeWL) : Create custom passoword by crawling a website.
- [username_generator](https://github.com/shroudri/username_generator)
# Custom Wordlists
### [crunch](https://github.com/jim3ma/crunch)
- Generates wordlist
```bash
crunch <min> <max> [charset] -o outfile.txt
# eg
crunch 6 6 abcd -o wordlist.txt
# Special chars
# @ - lower case alpha characters 
# , - upper case alpha characters
# % - numeric characters
# ^ - special characters including space
```
### [Cupp](https://github.com/Mebus/cupp)
- Common user password profiler
- interactive tool to generated password list based on person.
```bash
python3 cupp.py -i
```
