Obtener el SID del dominio con GETPAC

```shell 
─$ getPac.py -targetUser administrator scrm.local/ksimpson:ksimpson
Impacket v0.13.0.dev0+20250611.105641.0612d078 - Copyright Fortra, LLC and its affiliated companies 

KERB_VALIDATION_INFO 
LogonTime:                      
    dwLowDateTime:                   2782757020 
    dwHighDateTime:                  31227582 
LogoffTime:                     
    dwLowDateTime:                   4294967295 
    dwHighDateTime:                  2147483647 
KickOffTime:                    
    dwLowDateTime:                   4294967295 
    dwHighDateTime:                  2147483647 
PasswordLastSet:                
    dwLowDateTime:                   2585823167 
    dwHighDateTime:                  30921784 
PasswordCanChange:              
    dwLowDateTime:                   3297396671 
    dwHighDateTime:                  30921985 
PasswordMustChange:             
    dwLowDateTime:                   4294967295 
    dwHighDateTime:                  2147483647 
EffectiveName:                   'administrator' 
FullName:                        '' 
LogonScript:                     '' 
ProfilePath:                     '' 
HomeDirectory:                   '' 
HomeDirectoryDrive:              '' 
LogonCount:                      261 
BadPasswordCount:                0 
UserId:                          500 
PrimaryGroupId:                  513 
GroupCount:                      5 
GroupIds:                       
    [
         
        RelativeId:                      513 
        Attributes:                      7 ,
         
        RelativeId:                      512 
        Attributes:                      7 ,
         
        RelativeId:                      520 
        Attributes:                      7 ,
         
        RelativeId:                      518 
        Attributes:                      7 ,
         
        RelativeId:                      519 
        Attributes:                      7 ,
    ] 
UserFlags:                       544 
UserSessionKey:                 
    Data:                            b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00' 
LogonServer:                     'DC1' 
LogonDomainName:                 'SCRM' 
LogonDomainId:                  
    Revision:                        1 
    SubAuthorityCount:               4 
    IdentifierAuthority:             b'\x00\x00\x00\x00\x00\x05' 
    SubAuthority:                   
        [
             21,
             2743207045,
             1827831105,
             2542523200,
        ] 
LMKey:                           b'\x00\x00\x00\x00\x00\x00\x00\x00' 
UserAccountControl:              16912 
SubAuthStatus:                   0 
LastSuccessfulILogon:           
    dwLowDateTime:                   0 
    dwHighDateTime:                  0 
LastFailedILogon:               
    dwLowDateTime:                   0 
    dwHighDateTime:                  0 
FailedILogonCount:               0 
Reserved3:                       0 
SidCount:                        1 
ExtraSids:                      
    [
         
        Sid:                            
            Revision:                        1 
            SubAuthorityCount:               1 
            IdentifierAuthority:             b'\x00\x00\x00\x00\x00\x12' 
            SubAuthority:                   
                [
                     2,
                ] 
        Attributes:                      7 ,
    ] 
ResourceGroupDomainSid:         
    Revision:                        1 
    SubAuthorityCount:               4 
    IdentifierAuthority:             b'\x00\x00\x00\x00\x00\x05' 
    SubAuthority:                   
        [
             21,
             2743207045,
             1827831105,
             2542523200,
        ] 
ResourceGroupCount:              1 
ResourceGroupIds:               
    [
         
        RelativeId:                      572 
        Attributes:                      536870919 ,
    ] 
Domain SID: S-1-5-21-2743207045-1827831105-2542523200

 0000   10 00 00 00 63 02 4D F2  2C 8B 97 15 2B 69 A2 92   ....c.M.,...+i..

```


Obtener Domain SID con Impacket 

```shell
ldapsearch -H ldap://dc1.scrm.local -D 'scrm.local\ksimpson' -w 'ksimpson' -Y GSSAPI -b "cn=users,dc=scrm,dc=local" | grep -i "objectSid::" | cut -d ":" -f3

SASL/GSSAPI authentication started
SASL username: ksimpson@SCRM.LOCAL
SASL SSF: 256
SASL data security layer installed.
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9gEAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAwIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBAIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBgIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBwIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBQIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAAIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAQIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAgIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCAIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXKQIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXOwIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXPAIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCQIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX8gEAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCgIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDQIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDgIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDwIAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXTQQAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXTgQAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9AEAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9QEAAA==
 AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXUAYAAA==
```

```shell
ldapsearch -H ldap://dc1.scrm.local -U 'ksimpson' -w 'ksimpson' -b "cn=users,dc=scrm,dc=local"  | grep -i sid

SASL/GSS-SPNEGO authentication started
SASL username: ksimpson@SCRM.LOCAL
SASL SSF: 256
SASL data security layer installed.
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9gEAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAwIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBAIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBgIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBwIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXBQIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAAIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAQIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXAgIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCAIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXKQIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXOwIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXPAIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCQIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX8gEAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXCgIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDQIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDgIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXDwIAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXTQQAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXTgQAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9AEAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uX9QEAAA==
objectSid:: AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXUAYAAA==
```



``` python 
import struct
import base64
import sys

def convert(binary):
    version = struct.unpack('B', binary[0:1])[0]
    assert version == 1, version

    length = struct.unpack('B', binary[1:2])[0]

    authority = struct.unpack('>Q', b'\x00\x00' + binary[2:8])[0]

    string = 'S-%d-%d' % (version, authority)

    binary = binary[8:]
    assert len(binary) == 4 * length

    for i in range(length):
        value = struct.unpack('<L', binary[4*i:4*(i+1)])[0]
        string += '-%d' % value

    return string

b64Sid = sys.argv[1]
sid = base64.b64decode(b64Sid)
print(convert(sid))
```

``` shell 
┌──(kali㉿kali)-[~/machines/Scrambled/content]
└─$ python sidtostring.py AQUAAAAAAAUVAAAAhQSCo0F98mxA04uXUAYAAA==
S-1-5-21-2743207045-1827831105-2542523200-1616
```


