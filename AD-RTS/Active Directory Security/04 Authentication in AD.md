#### Authentication in Active Directory

- Kerberos : Primary authentication protocol
- Client : User Workstations
- Key Distribution Center (KDC) : Domain Controller
- Service : Resource Client would like to access

NTLM : Older protocol, used for backward compatibility or non-domain scenarios

# 🧠 Authorization in Active Directory (AD)

## 🔸 Concepto General
La **autorización en Active Directory (AD)** determina **quién puede acceder a qué recursos** dentro del dominio y **qué acciones puede realizar**.  
Se basa en tres elementos principales:

---

## 1️⃣ Access Control Lists (ACLs)
- **Definición:** Listas de control de acceso que definen **permisos sobre objetos y recursos** (usuarios, carpetas, GPOs, etc.).
- **Estructura interna:** Compuestas por varias **Access Control Entries (ACEs)**, cada una define un permiso para un usuario o grupo.
- **Ejemplo:** Permitir a un usuario leer un objeto, pero denegar la modificación.

---

## 2️⃣ Security Descriptors
- **Función:** Cada objeto en AD (usuario, grupo, OU, etc.) tiene un **Security Descriptor** que contiene las **ACLs**.
- **Incluye:**
  - **Owner:** Propietario del objeto.
  - **DACL (Discretionary ACL):** Define quién tiene permisos.
  - **SACL (System ACL):** Define qué acciones generan auditoría.

---

## 3️⃣ Security Groups
- **Uso principal:** Asignar permisos de forma eficiente mediante grupos.
- **Ventaja:** Permite administrar permisos a nivel de grupo, evitando configuraciones individuales.
- **Tipos comunes:**
  - **Domain Local:** Permisos dentro del dominio.
  - **Global:** Miembros del mismo dominio.
  - **Universal:** Miembros de múltiples dominios.

---

# 🎯 Targeted Domain Roles (Roles de Dominio Críticos)

| **Grupo** | **Privilegio** | **Riesgo o Abuso Potencial** |
|------------|----------------|------------------------------|
| **Enterprise Admins** | Control total a nivel de *forest* (todos los dominios) | Compromiso total del bosque AD |
| **Domain Admins** | Control total a nivel de *dominio* | Crear cuentas, acceder a DCs, establecer persistencia |
| **Administrators (Built-in)** | Control local y de dominio | Control total sobre máquinas donde esté presente |
| **Schema Admins** | Modificar el esquema AD (a nivel forestal) | Persistencia sigilosa y permanente |
| **Group Policy Creator Owners** | Crear y enlazar GPOs | Inyectar políticas maliciosas o persistencia |

---

# ⚙️ Conceptos Clave de Autorización Avanzada

### 🔹 Delegation of Control
Permite asignar permisos específicos a usuarios o grupos para administrar ciertos objetos sin otorgar privilegios globales.

### 🔹 Authorization Paths
Representan las rutas de permisos o delegaciones que conectan objetos; útiles para identificar posibles escaladas de privilegios.

### 🔹 Privilege Escalation Routes
Rutas que un atacante podría usar para elevar privilegios aprovechando configuraciones erróneas en ACLs o grupos.

---

# 🧩 Buenas Prácticas

1. Usar **grupos de seguridad** en lugar de permisos directos.
2. Revisar periódicamente **ACLs y delegaciones**.
3. Monitorear los **grupos privilegiados** (Domain Admins, Enterprise Admins, etc.).
4. Implementar **auditorías (SACL)** para detectar accesos sospechosos.
5. Minimizar los miembros de roles críticos.
6. Aplicar el principio de **menor privilegio (PoLP)** en toda la infraestructura.

---

> 💡 **Resumen:**  
> La autorización en AD se basa en las ACLs y los grupos de seguridad.  
> Una mala configuración puede generar rutas de escalada de privilegios y comprometer todo el dominio.  
> Controlar delegaciones, roles críticos y auditorías es esencial para mantener la seguridad del Directorio Activo.
