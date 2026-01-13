
`getPac.py` es una herramienta que **solicita un PAC (Privilege Attribute Certificate)** a un **Domain Controller** de Active Directory **en nombre de un usuario**, usando credenciales válidas.

El **PAC** es un componente crítico de Kerberos que contiene:

- SID del usuario
    
- Grupos a los que pertenece
    
- Privilegios (por ejemplo, si es Domain Admin)
    
- Información usada por el DC para autorizar accesos
    

En auditorías, se usa para:

- Ver **qué privilegios reales** tiene una cuenta
    
- Comprobar **delegaciones, trusts o configuraciones Kerberos**
    
- Preparar ataques avanzados (Golden Ticket, Silver Ticket, S4U, etc.)

El comando **solicita al Domain Controller el PAC (Privilege Attribute Certificate) del usuario _administrator_** utilizando **autenticación Kerberos con las credenciales del usuario `ksimpson`**, con el fin de **obtener y analizar los privilegios y grupos asociados a ese usuario en Active Directory**.

``` shell 
getPac.py -targetUser administrator scrm.local/ksimpson:ksimpson
```

|Elemento|Función|
|---|---|
|`getPac.py`|Solicita PAC vía Kerberos|
|`-targetUser administrator`|Usuario objetivo|
|`scrm.local/ksimpson:ksimpson`|Credenciales usadas|
|Resultado|Información de privilegios|
```shell
 getPac.py -targetUser administrator scrm.local/ksimpson:ksimpson
Impacket v0.13.0.dev0+20250611.105641.0612d078 - Copyright Fortra, LLC and its affiliated companies 

KERB_VALIDATION_INFO 
LogonTime:                      
    dwLowDateTime:                   3917716618 
    dwHighDateTime:                  31227130 
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

 0000   10 00 00 00 7F 9A 67 42  54 7D 75 97 13 48 F1 7E   ......gBT}u..H.~
```
