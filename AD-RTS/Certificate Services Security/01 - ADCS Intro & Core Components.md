# Active Directory Certificate Services (ADCS)

## Descripción General

**Active Directory Certificate Services (ADCS)** es la implementación de **infraestructura de clave pública (PKI)** de **Microsoft**.  
Permite emitir, validar, renovar y revocar certificados digitales utilizados para asegurar la identidad y las comunicaciones dentro de una red empresarial.

ADCS se integra profundamente con **Active Directory (AD)** para proporcionar automatización, administración centralizada y políticas de seguridad consistentes.

---

## Importancia de ADCS

- Proporciona confianza digital dentro de la organización.  
- Habilita autenticación fuerte (usuarios, equipos, servicios).  
- Permite cifrado de datos y firmas digitales seguras.  
- Facilita la gestión automática de certificados mediante autoinscripción (autoenrollment).  
- Es fundamental para servicios como:
  - SSL/TLS (HTTPS, VPNs, LDAPS)
  - Autenticación basada en certificados
  - Smart Cards y autenticación multifactor
  - Cifrado de correos electrónicos (S/MIME)

---

## ADCS Core Components

- **Certification Authority (CA):** Emite, revoca y administra certificados digitales.  
- **Certificate Templates:** Definen las propiedades y políticas de los certificados emitidos por la CA.  
- **Certificate Authority Web Enrollment:** Rol opcional que permite solicitudes de certificados a través de una interfaz web.  
- **Certificate Revocation List (CRL):** Lista publicada por la CA que contiene los certificados que ya no son válidos y no deben ser confiados.  
- **Certification Distribution Point (CDP):** Ubicación donde los clientes obtienen la CRL para verificar si un certificado ha sido revocado.

---

## Componentes Principales de ADCS

| Componente | Descripción | Función principal |
|-------------|--------------|-------------------|
| Certification Authority (CA) | Servidor que emite y gestiona certificados digitales. | Autoriza y firma certificados. |
| CA Web Enrollment | Interfaz web para solicitar y emitir certificados manualmente. | Permite solicitudes desde navegadores. |
| Online Responder (OCSP) | Servicio para verificar el estado de un certificado en tiempo real. | Proporciona respuestas inmediatas sobre revocaciones. |
| Network Device Enrollment Service (NDES) | Permite que dispositivos de red obtengan certificados sin un usuario interactivo. | Integración con routers, switches, IoT. |
| Certificate Enrollment Web Service (CES) | Permite inscripción de certificados a través de HTTP/HTTPS. | Útil en entornos sin conexión directa a AD. |
| Certificate Enrollment Policy Web Service (CEP) | Define políticas de emisión y renovación de certificados. | Gestiona reglas y restricciones de emisión. |

---

## Tipos de Autoridades de Certificación (CA)

1. **Root CA (Autoridad raíz)**  
   - Punto de confianza principal.  
   - Generalmente se mantiene **offline** por seguridad.  
   - Firma las CAs subordinadas.

2. **Subordinate / Issuing CA (CA emisora)**  
   - Emite certificados a usuarios, equipos y servicios.  
   - Opera bajo la Root CA.

3. **Enterprise CA**  
   - Integrada con Active Directory.  
   - Usa políticas de grupo (GPO) para autoinscripción.  

4. **Standalone CA**  
   - No requiere integración con AD.  
   - Ideal para entornos aislados o externos.

---

## Flujo Básico de Emisión de Certificados

1. El usuario o equipo genera una solicitud (CSR).  
2. La CA valida la identidad y emite el certificado.  
3. El certificado se instala y se utiliza para autenticación, cifrado o firma.  
4. ADCS gestiona renovaciones y revocaciones según las políticas establecidas.

---

## Casos de Uso Comunes

- Certificados para servidores HTTPS (IIS, Exchange, VPN).  
- Autenticación Kerberos con certificados.  
- Cifrado de archivos mediante EFS.  
- Firmas digitales de software o documentos.  
- Integración con MDM o Intune para dispositivos móviles.

---

## Beneficios Clave

- Seguridad centralizada y escalable.  
- Integración con políticas de Active Directory.  
- Reducción del riesgo de suplantación de identidad.  
- Automatización mediante autoinscripción.  
- Cumplimiento de normativas y estándares (ISO 27001, NIST, GDPR).


# 🧠 AD CS – Rutas, Comandos y Notas Rápidas

## 📘 Introducción
**Active Directory Certificate Services (ADCS)**  
Infraestructura de Clave Pública (PKI) de Microsoft para emitir, validar y administrar certificados digitales.  
Emite certificados SSL/TLS, de autenticación, firma de código, y autenticación por smartcard.

---

## 🧩 Componentes Clave
- **Certification Authority (CA):** emite, revoca y gestiona certificados.
- **Certificate Templates:** define las propiedades, políticas y permisos de emisión.
- **CRL (Certificate Revocation List):** lista de certificados revocados.
- **CDP (CRL Distribution Point):** ubicación donde los clientes descargan las CRL.
- **AIA (Authority Information Access):** ruta donde los clientes obtienen certificados de CA intermedias.
- **NTAuthCertificates:** almacena certificados de CA de confianza para autenticación.

---

## 🗂 Consolas y Herramientas Principales

| Herramienta                          | Comando / Ruta                                  | Uso                                                               |
| ------------------------------------ | ----------------------------------------------- | ----------------------------------------------------------------- |
| **Consola de la CA**                 | `certsrv.msc`                                   | Administración de certificados emitidos, revocados y CRL.         |
| **Enterprise PKI (PKIView)**         | `pkiview.msc`                                   | Verifica salud de la PKI (CRL, AIA, CA raíz, contenedores en AD). |
| **Plantillas de certificado**        | `certtmpl.msc`                                  | Gestión de propiedades, permisos y EKU de las plantillas.         |
| **Consola de certificados**          | `mmc.exe` → *Add Snap-in → Certificates*        | Ver certificados de usuario o equipo local.                       |
| **Edición de objetos PKI en AD**     | `adsiedit.msc` → *Configuration Naming Context* | Modificación manual de AIA, CDP, NTAuth, etc.                     |
| **Contenedores PKI (vista gráfica)** | `pkiview.msc` → *Manage AD Containers*          | Acceso directo a los contenedores PKI en AD.                      |

---

## 📁 Contenedores PKI en Active Directory

| Contenedor | Ruta en AD | Contenido |
|-------------|-------------|-----------|
| **AIA** | `CN=AIA,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | Certificados de CA raíz o intermedias. |
| **CDP** | `CN=CDP,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | Publicación de CRL. |
| **Enrollment Services** | `CN=Enrollment Services,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | CA registradas disponibles en el dominio. |
| **Certificate Templates** | `CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | Plantillas de certificados. |
| **OID** | `CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | Identificadores de objeto (EKU, políticas). |
| **NTAuthCertificates** | `CN=NTAuthCertificates,CN=Public Key Services,CN=Services,CN=Configuration,DC=...` | Certificados de CA confiables para autenticación. |

📌 *Puedes acceder mediante `adsiedit.msc` o en PKIView → Manage AD Containers.*

---

## 🧰 Comandos Útiles de CertUtil

| Comando | Descripción |
|----------|-------------|
| `certutil -CAinfo` | Información general de la CA. |
| `certutil -store My` | Muestra los certificados en el almacén “Personal”. |
| `certutil -dump cert.cer` | Muestra detalles de un certificado. |
| `certutil -verify -urlfetch cert.cer` | Valida cadena de confianza, CRL y AIA. |
| `certutil -crl` | Publica una nueva CRL. |
| `certutil -getreg` | Muestra configuración del registro de la CA. |
| `certutil -setreg CA\CRLPeriodUnits 1`<br>`certutil -setreg CA\CRLPeriod "Weeks"` | Configura la frecuencia de publicación de CRL. |
| `certutil -dspublish -f cert.cer NTAuthCA` | Publica un certificado de CA en NTAuthCertificates. |
| `certutil -repairstore My <Thumbprint>` | Reasocia certificado con su clave privada. |
| `certutil -viewstore` | Abre vista gráfica del almacén de certificados. |

---

## 🔍 Consultas Comunes de Certificados

| Acción | Dónde / Herramienta | Descripción |
|--------|---------------------|--------------|
| Ver certificados emitidos por la CA | `certsrv.msc` → *Issued Certificates* | Lista de certificados emitidos. |
| Ver certificados revocados | `certsrv.msc` → *Revoked Certificates* | Lista de certificados revocados. |
| Revisar certificados de confianza en AD | `pkiview.msc` → *Manage AD Containers → NTAuthCertificates* | Confirma certificados de CA de confianza. |
| Ver plantillas habilitadas | `certsrv.msc` → *Certificate Templates* | Plantillas publicadas para emisión. |
| Ver certificados locales | `mmc.exe` + Snap-in *Certificates (Local Computer)* | Certificados instalados en el sistema. |

---

## 🧾 Rutas y Archivos Locales Importantes

| Ruta | Descripción |
|------|--------------|
| `C:\Windows\System32\CertSrv\CertEnroll` | Archivos de CRL y certificados emitidos. |
| `C:\Windows\System32\CertLog` | Base de datos y logs de la CA. |
| `%SystemRoot%\System32\certsrv` | Archivos del servicio de certificación. |
| `%ProgramData%\Microsoft\Crypto\RSA\MachineKeys` | Claves privadas del equipo. |
| `%APPDATA%\Microsoft\SystemCertificates\My\Certificates` | Certificados de usuario. |

---

## 🧱 Permisos sobre Plantillas

> ⚠️ Permisos incorrectos = riesgo de **theft**, **impersonation** o abuso de plantillas.

En `certtmpl.msc` → plantilla → pestaña **Security**:
- **Domain Admins / Enterprise Admins:** Full Control  
- **Domain Users:** Read / Enroll (si aplica)  
- **Autoenroll:** solo para grupos autorizados  

**PowerShell:**
```powershell
certutil -dstemplate
Get-CATemplate | Select DisplayName, Flags, Permissions
