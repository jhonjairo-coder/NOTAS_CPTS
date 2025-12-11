[ LINK] https://github.com/Flangvik/SharpCollection/tree/master


```bash
bloodhound-python -d fluffy.htb -u 'p.agila' -p 'prometheusx-303' -dc 'dc01.fluffy.htb' -c all -ns 10.129.21.100
```

```bash
bloodhound-python -c all -u 'judith.mader' -p 'judith09' -ns 10.129.231.186 -d certified.htb
```

```bash
netexec ldap certified.htb -u  judith.mader -p judith09 --bloodhound --collection All --dns-server $IP 
```


``` shell
┌──(kali㉿kali)-[~/machines/Timelapse/content/test_dos]
└─$ sudo /usr/bin/bloodhound_community/bloodhound-cli containers start
```
