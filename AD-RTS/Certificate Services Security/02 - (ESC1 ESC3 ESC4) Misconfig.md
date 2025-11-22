# 🧩 ¿Qué significa ESC1?  
*(Enterprise Certificate Services Misconfiguration 1)*

## 🔠 Significado de las siglas
**ESC** = **Enterprise Certificate Service Configuration** (también referenciado como *Enterprise Security Configuration* en algunos documentos).  
El término fue definido por **SpecterOps** en su investigación **“Certified Pre-Owned”**, donde catalogaron una serie de **vulnerabilidades y malas configuraciones** en infraestructuras de certificados dentro de Active Directory (AD CS).

Cada **ESC (1, 2, 3, …)** identifica un **tipo específico de misconfiguration** o abuso posible en un entorno PKI de Microsoft.

---

## 📚 Contexto
Active Directory Certificate Services (AD CS) es la implementación de PKI en Windows Server.  
Una CA (Certification Authority) emite certificados a usuarios, equipos o servicios para autenticación, cifrado o firma digital.

Cuando la configuración no se controla adecuadamente, los atacantes pueden abusar de ciertas plantillas o permisos de la CA para **emitirse certificados que les otorgan autenticación como cuentas privilegiadas** — incluso *Domain Admins*.

---

## ⚠️ ESC1 — “Template Misconfiguration”
**ESC1** corresponde a la **primera categoría** de vulnerabilidad identificada por SpecterOps.  
Se produce cuando una **plantilla de certificado** tiene combinadas tres condiciones peligrosas:

| Condición                                                               | Descripción                                                            | Riesgo                                                  |
| ----------------------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------- |
| **1️⃣ Permisos de “Enroll”** accesibles a usuarios no privilegiados     | “Domain Users” o “Authenticated Users” pueden solicitar certificados   | Cualquier usuario del dominio puede emitir certificados |
| **2️⃣ Subject Name → “Supply in the request”**                          | El solicitante puede especificar manualmente el nombre (Subject o SAN) | Puede suplantar identidades (p. ej. Administrator)      |
| **3️⃣ EKUs de autenticación** (Client Authentication / Smartcard Logon) | El certificado permite iniciar sesión en AD                            | Permite autenticarse como otra cuenta                   |

### 🔁 Resultado
> Un usuario de bajo privilegio puede **solicitar un certificado en nombre de otra cuenta**, la CA lo emite, y el atacante usa ese certificado para autenticarse vía Kerberos (PKINIT) como el usuario objetivo (incluso Administrator).

---

## 🔍 Flujo resumido de explotación ESC1

1. **Enumerar plantillas vulnerables**
   ```bash
   Certify.exe find /vulnerable
```

# ESC3 – Enrollment Agent y Plantillas con Privilegios Elevados

## Descripción
ESC3 ocurre cuando una plantilla de certificado permite a usuarios con pocos privilegios solicitar certificados que incluyen EKUs avanzados, como:

- Enrollment Agent
- Smartcard Logon
- Client Authentication

Estas plantillas pueden permitir emitir certificados en nombre de otros usuarios, lo que habilita ataques de suplantación.

---

## Condiciones que producen ESC3
- La plantilla contiene EKUs avanzados (por ejemplo, Enrollment Agent).
- Usuarios comunes poseen permisos ENROLL sobre la plantilla.
- La CA emite certificados sin requerir aprobación manual.

---

## Escenario de ataque
1. Un usuario de bajo privilegio identifica una plantilla vulnerable.
2. Solicita un certificado con el EKU Enrollment Agent.
3. Usa ese certificado para solicitar uno nuevo en nombre de una cuenta privilegiada.
4. La CA emite el certificado sin validación adecuada.
5. El atacante se autentica como el usuario privilegiado mediante PKINIT.

---

## Impacto
- Suplantación de cualquier cuenta del dominio.
- Escalada a Domain Admin.
- Emisión de certificados válidos y confiables.
- Persistencia mediante certificados.

---

# ESC4 – Permisos WRITE sobre Plantillas de Certificado

## Descripción
ESC4 ocurre cuando un usuario de bajo privilegio tiene permisos WRITE sobre una plantilla de certificado. Esto le permite modificar parámetros críticos, como:

- EKUs
- Subject Name o SAN
- Permisos de ENROLL
- Reglas de aprobación

Con este control, el atacante puede convertir la plantilla en una vulnerable de tipo ESC1.

---

## Condiciones que producen ESC4
- El atacante posee permisos WRITE sobre la plantilla.
- Falta de controles RBAC adecuados.
- La CA emite certificados automáticamente sin aprobación manual.

---

## Escenario de ataque
1. El atacante identifica que puede modificar la plantilla.
2. Cambia la plantilla para permitir SAN arbitrarios, EKUs críticos y permisos ENROLL para sí mismo.
3. Solicita un certificado usando la plantilla modificada.
4. Obtiene un certificado que le permite hacerse pasar por cuentas privilegiadas.
5. Se autentica como Administrator, krbtgt u otra cuenta privilegiada.

---

## Impacto
- Modificación silenciosa de plantillas.
- Escalada a Domain Admin.
- Persistencia mediante certificados emitidos.
- Abuso de PKI para evadir controles y monitoreo.

---

# Resumen Comparativo

| ESC  | Tipo de fallo                                   | Qué permite                                   | Riesgo    |
|------|--------------------------------------------------|-----------------------------------------------|-----------|
| ESC3 | EKUs avanzados + permisos ENROLL accesibles      | Emitir certificados por otros usuarios         | Alto      |
| ESC4 | Permisos WRITE sobre la plantilla                | Modificar la plantilla y convertirla en ESC1   | Muy alto  |

