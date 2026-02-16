# Pivoting con SSH y Proxychains — Notas completas (CPTS)

## ¿Qué es Pivoting?
Pivoting es una técnica utilizada en pentesting para emplear un host intermedio (pivot host) como puente hacia redes internas que no son accesibles directamente desde la máquina atacante. Normalmente el pivot host tiene acceso a dos o más redes (host dual-homed). SSH permite realizar pivoting de forma nativa mediante túneles cifrados, y proxychains permite forzar herramientas a usar esos túneles.

## Escenario típico
[Kali / Attacker] -> SSH -> [Pivot Host: 192.168.80.10 / 192.168.98.15] -> [Red interna 192.168.98.0/24]

## Tipos de Port Forwarding en SSH
- Local Port Forwarding (-L)
- Remote Port Forwarding (-R)
- Dynamic Port Forwarding (-D) -> SOCKS (principal para pivoting)

## Dynamic Port Forwarding (SOCKS)
ssh -N -D <PUERTO_LOCAL> usuario@<pivot_host>

Ejemplo:
ssh -N -D 1080 privilege@192.168.80.10

## Proxychains (configuración recomendada)
dynamic_chain
proxy_dns
socks5 127.0.0.1 1080

## Puerto 9050
Se usa por convención histórica (TOR). No es obligatorio.

## Validación del Pivot
proxychains nc 192.168.98.15 22
proxychains nmap -sn 192.168.98.0/24

## Errores comunes
Connection refused indica servicio inexistente, no fallo del túnel.

## Enumeración
proxychains nmap -sT -Pn 192.168.98.0/24

## Limitaciones
- No ICMP
- No UDP general
- Solo TCP

## Pivoting multi-salto
Encadenar proxies SOCKS para llegar a redes más profundas.

## Frase clave
SSH Dynamic Port Forwarding permite pivoting mediante SOCKS; proxychains fuerza herramientas a usar el túnel.