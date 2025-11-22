
|**Server Type**|**Description**|
|---|---|
|`DNS Root Server`|The root servers of the DNS are responsible for the top-level domains (`TLD`). As the last instance, they are only requested if the name server does not respond. Thus, a root server is a central interface between users and content on the Internet, as it links domain and IP address. The [Internet Corporation for Assigned Names and Numbers](https://www.icann.org/) (`ICANN`) coordinates the work of the root name servers. There are `13` such root servers around the globe.|
|`Authoritative Nameserver`|Authoritative name servers hold authority for a particular zone. They only answer queries from their area of responsibility, and their information is binding. If an authoritative name server cannot answer a client's query, the root name server takes over at that point. Based on the country, company, etc., authoritative nameservers provide answers to recursive DNS nameservers, assisting in finding the specific web server(s).|
|`Non-authoritative Nameserver`|Non-authoritative name servers are not responsible for a particular DNS zone. Instead, they collect information on specific DNS zones themselves, which is done using recursive or iterative DNS querying.|
|`Caching DNS Server`|Caching DNS servers cache information from other name servers for a specified period. The authoritative name server determines the duration of this storage.|
|`Forwarding Server`|Forwarding servers perform only one function: they forward DNS queries to another DNS server.|
|`Resolver`|Resolvers are not authoritative DNS servers but perform name resolution locally in the computer or router.|

|**DNS Record**|**Description**|
|---|---|
|`A`|Returns an IPv4 address of the requested domain as a result.|
|`AAAA`|Returns an IPv6 address of the requested domain.|
|`MX`|Returns the responsible mail servers as a result.|
|`NS`|Returns the DNS servers (nameservers) of the domain.|
|`TXT`|This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam.|
|`CNAME`|This record serves as an alias for another domain name. If you want the domain www.hackthebox.eu to point to the same IP as hackthebox.eu, you would create an A record for hackthebox.eu and a CNAME record for www.hackthebox.eu.|
|`PTR`|The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.|
|`SOA`|Provides information about the corresponding DNS zone and email address of the administrative contact.|

```shell-session
jhonmorales@htb[/htb]$ dig soa www.inlanefreight.com

; <<>> DiG 9.16.27-Debian <<>> soa www.inlanefreight.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15876
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;www.inlanefreight.com.         IN      SOA

;; AUTHORITY SECTION:
inlanefreight.com.      900     IN      SOA     ns-161.awsdns-20.com. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400

;; Query time: 16 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Thu Jan 05 12:56:10 GMT 2023
;; MSG SIZE  rcvd: 128
```


#### Local DNS Configuration

```shell-session
root@bind9:~# cat /etc/bind/named.conf.local

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "domain.com" {
    type master;
    file "/etc/bind/db.domain.com";
    allow-update { key rndc-key; };
};
```

#### Zone Files

```shell-session
root@bind9:~# cat /etc/bind/db.domain.com

;
; BIND reverse data file for local loopback interface
;
$ORIGIN domain.com
$TTL 86400
@     IN     SOA    dns1.domain.com.     hostmaster.domain.com. (
                    2001062501 ; serial
                    21600      ; refresh after 6 hours
                    3600       ; retry after 1 hour
                    604800     ; expire after 1 week
                    86400 )    ; minimum TTL of 1 day

      IN     NS     ns1.domain.com.
      IN     NS     ns2.domain.com.

      IN     MX     10     mx.domain.com.
      IN     MX     20     mx2.domain.com.

             IN     A       10.129.14.5

server1      IN     A       10.129.14.5
server2      IN     A       10.129.14.7
ns1          IN     A       10.129.14.2
ns2          IN     A       10.129.14.3

ftp          IN     CNAME   server1
mx           IN     CNAME   server1
mx2          IN     CNAME   server2
www          IN     CNAME   server2
```

#### Reverse Name Resolution Zone Files

```shell-session
root@bind9:~# cat /etc/bind/db.10.129.14

;
; BIND reverse data file for local loopback interface
;
$ORIGIN 14.129.10.in-addr.arpa
$TTL 86400
@     IN     SOA    dns1.domain.com.     hostmaster.domain.com. (
                    2001062501 ; serial
                    21600      ; refresh after 6 hours
                    3600       ; retry after 1 hour
                    604800     ; expire after 1 week
                    86400 )    ; minimum TTL of 1 day

      IN     NS     ns1.domain.com.
      IN     NS     ns2.domain.com.

5    IN     PTR    server1.domain.com.
7    IN     MX     mx.domain.com.
...SNIP...
```

## Footprinting the Service

#### DIG - NS Query
```shell-session
jhonmorales@htb[/htb]$ dig ns inlanefreight.htb @10.129.14.128

; <<>> DiG 9.16.1-Ubuntu <<>> ns inlanefreight.htb @10.129.14.128
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 45010
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: ce4d8681b32abaea0100000061475f73842c401c391690c7 (good)
;; QUESTION SECTION:
;inlanefreight.htb.             IN      NS

;; ANSWER SECTION:
inlanefreight.htb.      604800  IN      NS      ns.inlanefreight.htb.

;; ADDITIONAL SECTION:
ns.inlanefreight.htb.   604800  IN      A       10.129.34.136

;; Query time: 0 msec
;; SERVER: 10.129.14.128#53(10.129.14.128)
;; WHEN: So Sep 19 18:04:03 CEST 2021
;; MSG SIZE  rcvd: 107
```

#### DIG - Version Query

```shell-session
jhonmorales@htb[/htb]$ dig CH TXT version.bind 10.129.120.85

; <<>> DiG 9.10.6 <<>> CH TXT version.bind
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 47786
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; ANSWER SECTION:
version.bind.       0       CH      TXT     "9.10.6-P1"

;; ADDITIONAL SECTION:
version.bind.       0       CH      TXT     "9.10.6-P1-Debian"

;; Query time: 2 msec
;; SERVER: 10.129.120.85#53(10.129.120.85)
;; WHEN: Wed Jan 05 20:23:14 UTC 2023
;; MSG SIZE  rcvd: 101
```

#### DIG - ANY Query

```shell-session
jhonmorales@htb[/htb]$ dig any inlanefreight.htb @10.129.14.128

; <<>> DiG 9.16.1-Ubuntu <<>> any inlanefreight.htb @10.129.14.128
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 7649
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 5, AUTHORITY: 0, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 064b7e1f091b95120100000061476865a6026d01f87d10ca (good)
;; QUESTION SECTION:
;inlanefreight.htb.             IN      ANY

;; ANSWER SECTION:
inlanefreight.htb.      604800  IN      TXT     "v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com include:_spf.atlassian.net ip4:10.129.124.8 ip4:10.129.127.2 ip4:10.129.42.106 ~all"
inlanefreight.htb.      604800  IN      TXT     "atlassian-domain-verification=t1rKCy68JFszSdCKVpw64A1QksWdXuYFUeSXKU"
inlanefreight.htb.      604800  IN      TXT     "MS=ms97310371"
inlanefreight.htb.      604800  IN      SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
inlanefreight.htb.      604800  IN      NS      ns.inlanefreight.htb.

;; ADDITIONAL SECTION:
ns.inlanefreight.htb.   604800  IN      A       10.129.34.136

;; Query time: 0 msec
;; SERVER: 10.129.14.128#53(10.129.14.128)
;; WHEN: So Sep 19 18:42:13 CEST 2021
;; MSG SIZE  rcvd: 437
```

#### DIG - AXFR Zone Transfer

```shell-session
jhonmorales@htb[/htb]$ dig axfr inlanefreight.htb @10.129.14.128

; <<>> DiG 9.16.1-Ubuntu <<>> axfr inlanefreight.htb @10.129.14.128
;; global options: +cmd
inlanefreight.htb.      604800  IN      SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
inlanefreight.htb.      604800  IN      TXT     "MS=ms97310371"
inlanefreight.htb.      604800  IN      TXT     "atlassian-domain-verification=t1rKCy68JFszSdCKVpw64A1QksWdXuYFUeSXKU"
inlanefreight.htb.      604800  IN      TXT     "v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com include:_spf.atlassian.net ip4:10.129.124.8 ip4:10.129.127.2 ip4:10.129.42.106 ~all"
inlanefreight.htb.      604800  IN      NS      ns.inlanefreight.htb.
app.inlanefreight.htb.  604800  IN      A       10.129.18.15
internal.inlanefreight.htb. 604800 IN   A       10.129.1.6
mail1.inlanefreight.htb. 604800 IN      A       10.129.18.201
ns.inlanefreight.htb.   604800  IN      A       10.129.34.136
inlanefreight.htb.      604800  IN      SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
;; Query time: 4 msec
;; SERVER: 10.129.14.128#53(10.129.14.128)
;; WHEN: So Sep 19 18:51:19 CEST 2021
;; XFR size: 9 records (messages 1, bytes 520)
```

#### DIG - AXFR Zone Transfer - Internal

```shell-session
jhonmorales@htb[/htb]$ dig axfr internal.inlanefreight.htb @10.129.14.128

; <<>> DiG 9.16.1-Ubuntu <<>> axfr internal.inlanefreight.htb @10.129.14.128
;; global options: +cmd
internal.inlanefreight.htb. 604800 IN   SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
internal.inlanefreight.htb. 604800 IN   TXT     "MS=ms97310371"
internal.inlanefreight.htb. 604800 IN   TXT     "atlassian-domain-verification=t1rKCy68JFszSdCKVpw64A1QksWdXuYFUeSXKU"
internal.inlanefreight.htb. 604800 IN   TXT     "v=spf1 include:mailgun.org include:_spf.google.com include:spf.protection.outlook.com include:_spf.atlassian.net ip4:10.129.124.8 ip4:10.129.127.2 ip4:10.129.42.106 ~all"
internal.inlanefreight.htb. 604800 IN   NS      ns.inlanefreight.htb.
dc1.internal.inlanefreight.htb. 604800 IN A     10.129.34.16
dc2.internal.inlanefreight.htb. 604800 IN A     10.129.34.11
mail1.internal.inlanefreight.htb. 604800 IN A   10.129.18.200
ns.internal.inlanefreight.htb. 604800 IN A      10.129.34.136
vpn.internal.inlanefreight.htb. 604800 IN A     10.129.1.6
ws1.internal.inlanefreight.htb. 604800 IN A     10.129.1.34
ws2.internal.inlanefreight.htb. 604800 IN A     10.129.1.35
wsus.internal.inlanefreight.htb. 604800 IN A    10.129.18.2
internal.inlanefreight.htb. 604800 IN   SOA     inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400 2419200 604800
;; Query time: 0 msec
;; SERVER: 10.129.14.128#53(10.129.14.128)
;; WHEN: So Sep 19 18:53:11 CEST 2021
;; XFR size: 15 records (messages 1, bytes 664)
```

#### Subdomain Brute Forcing

```shell-session
jhonmorales@htb[/htb]$ for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done

ns.inlanefreight.htb.   604800  IN      A       10.129.34.136
mail1.inlanefreight.htb. 604800 IN      A       10.129.18.201
app.inlanefreight.htb.  604800  IN      A       10.129.18.15
```

#### DNSENUM

https://github.com/fwaeytens/dnsenum

```shell-session
jhonmorales@htb[/htb]$ dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb

dnsenum VERSION:1.2.6

-----   inlanefreight.htb   -----


Host's addresses:
__________________



Name Servers:
______________

ns.inlanefreight.htb.                    604800   IN    A        10.129.34.136


Mail (MX) Servers:
___________________



Trying Zone Transfers and getting Bind Versions:
_________________________________________________

unresolvable name: ns.inlanefreight.htb at /usr/bin/dnsenum line 900 thread 1.

Trying Zone Transfer for inlanefreight.htb on ns.inlanefreight.htb ...
AXFR record query failed: no nameservers


Brute forcing with /home/cry0l1t3/Pentesting/SecLists/Discovery/DNS/subdomains-top1million-110000.txt:
_______________________________________________________________________________________________________

ns.inlanefreight.htb.                    604800   IN    A        10.129.34.136
mail1.inlanefreight.htb.                 604800   IN    A        10.129.18.201
app.inlanefreight.htb.                   604800   IN    A        10.129.18.15
ns.inlanefreight.htb.                    604800   IN    A        10.129.34.136

...SNIP...
done.
```

#### 6.3 DNS Enum & Zone Transfer

##### Description

Step by step on establishing a secure VPN link to the AD-RTS lab and verifying access to target subnets.

DNS are the backbone of the entire network, let’s scan those and identify misconfigurations:

``` bash
nmap -sU -p 53 --open 10.5.2.0/24
```

``` bash
sudo nano /etc/resolv.conf  
#nameserver <NAT IP>  
nameserver 10.5.2.99
```


```bash

┌──(adrts)─(kali㉿kali)-[~/machines/adrts]
└─$ for i in {1..254}; do echo -n "10.5.2.$i "; dig +short -x 10.5.2.$i; done
10.5.2.1 10.5.2.2 dc01.telecore.ad.
10.5.2.3 10.5.2.4 10.5.2.5 10.5.2.6 10.5.2.7 10.5.2.8 PKI-Srv.telecore.ad.
10.5.2.9 10.5.2.10 10.5.2.11 10.5.2.12 10.5.2.13 10.5.2.14 10.5.2.15 10.5.2.16 Ex-Srv.telecore.ad.
10.5.2.17 10.5.2.18 10.5.2.19 10.5.2.20 10.5.2.21 10.5.2.22 SQL-Srv.telecore.ad.
10.5.2.23 10.5.2.24 10.5.2.25 10.5.2.26 10.5.2.27 10.5.2.28 10.5.2.29 10.5.2.30 10.5.2.31 10.5.2.32 10.5.2.33 10.5.2.34 10.5.2.35 10.5.2.36 10.5.2.37 10.5.2.38 10.5.2.39 10.5.2.40 10.5.2.41 10.5.2.42 10.5.2.43 10.5.2.44 10.5.2.45 10.5.2.46 10.5.2.47 10.5.2.48 10.5.2.49 10.5.2.50 10.5.2.51 10.5.2.52 10.5.2.53 10.5.2.54 10.5.2.55 10.5.2.56 10.5.2.57 10.5.2.58 10.5.2.59 10.5.2.60 10.5.2.61 10.5.2.62 10.5.2.63 10.5.2.64 10.5.2.65 10.5.2.66 10.5.2.67 10.5.2.68 10.5.2.69 10.5.2.70 10.5.2.71 10.5.2.72 10.5.2.73 10.5.2.74 10.5.2.75 10.5.2.76 10.5.2.77 10.5.2.78 10.5.2.79 10.5.2.80 10.5.2.81 10.5.2.82 10.5.2.83 10.5.2.84 10.5.2.85 10.5.2.86 10.5.2.87 10.5.2.88 10.5.2.89 10.5.2.90 10.5.2.91 10.5.2.92 10.5.2.93 10.5.2.94 10.5.2.95 10.5.2.96 10.5.2.97 10.5.2.98 10.5.2.99 10.5.2.100 10.5.2.101 10.5.2.102 10.5.2.103 10.5.2.104 10.5.2.105 10.5.2.106 10.5.2.107 10.5.2.108 10.5.2.109 10.5.2.110 10.5.2.111 ESXi-VD.telecore.ad.
10.5.2.112 10.5.2.113 10.5.2.114 10.5.2.115 10.5.2.116 10.5.2.117 10.5.2.118 10.5.2.119 10.5.2.120 10.5.2.121 10.5.2.122 10.5.2.123 10.5.2.124 10.5.2.125 10.5.2.126 10.5.2.127 10.5.2.128 10.5.2.129 10.5.2.130 10.5.2.131 10.5.2.132 10.5.2.133 10.5.2.134 10.5.2.135 10.5.2.136 10.5.2.137 10.5.2.138 10.5.2.139 10.5.2.140 10.5.2.141 10.5.2.142 10.5.2.143 10.5.2.144 10.5.2.145 10.5.2.146 10.5.2.147 10.5.2.148 10.5.2.149 10.5.2.150 10.5.2.151 10.5.2.152 10.5.2.153 10.5.2.154 10.5.2.155 10.5.2.156 10.5.2.157 10.5.2.158 10.5.2.159 10.5.2.160 10.5.2.161 10.5.2.162 10.5.2.163 10.5.2.164 10.5.2.165 10.5.2.166 10.5.2.167 10.5.2.168 10.5.2.169 10.5.2.170 10.5.2.171 10.5.2.172 10.5.2.173 10.5.2.174 10.5.2.175 10.5.2.176 10.5.2.177 10.5.2.178 10.5.2.179 10.5.2.180 10.5.2.181 10.5.2.182 10.5.2.183 10.5.2.184 10.5.2.185 10.5.2.186 10.5.2.187 10.5.2.188 10.5.2.189 10.5.2.190 10.5.2.191 10.5.2.192 10.5.2.193 10.5.2.194 10.5.2.195 10.5.2.196 10.5.2.197 10.5.2.198 10.5.2.199 10.5.2.200 10.5.2.201 10.5.2.202 10.5.2.203 10.5.2.204 10.5.2.205 10.5.2.206 10.5.2.207 10.5.2.208 10.5.2.209 10.5.2.210 10.5.2.211 10.5.2.212 10.5.2.213 10.5.2.214 10.5.2.215 10.5.2.216 10.5.2.217 10.5.2.218 10.5.2.219 10.5.2.220 10.5.2.221 10.5.2.222 10.5.2.223 10.5.2.224 10.5.2.225 10.5.2.226 10.5.2.227 10.5.2.228 10.5.2.229 10.5.2.230 10.5.2.231 10.5.2.232 10.5.2.233 10.5.2.234 10.5.2.235 10.5.2.236 10.5.2.237 10.5.2.238 10.5.2.239 10.5.2.240 10.5.2.241 10.5.2.242 10.5.2.243 10.5.2.244 10.5.2.245 10.5.2.246 10.5.2.247 10.5.2.248 10.5.2.249 10.5.2.250 10.5.2.251 10.5.2.252 10.5.2.253 10.5.2.254   
```


We obtained host mappings via PTR sweep. The domain name is “telecore.ad”. Next, we  
attempt a reverse zone transfer (AXFR)

``` bash
dig @10.5.2.99 2.5.10.in-addr.arpa AXFR

; <<>> DiG 9.20.9-1-Debian <<>> @10.5.2.99 2.5.10.in-addr.arpa AXFR
; (1 server found)
;; global options: +cmd
2.5.10.in-addr.arpa.    3600    IN      SOA     dc01.telecore.ad. hostmaster.telecore.ad. 6 900 600 86400 3600
2.5.10.in-addr.arpa.    3600    IN      NS      dc01.telecore.ad.
111.2.5.10.in-addr.arpa. 3600   IN      PTR     ESXi-VD.telecore.ad.
16.2.5.10.in-addr.arpa. 3600    IN      PTR     Ex-Srv.telecore.ad.
2.2.5.10.in-addr.arpa.  3600    IN      PTR     dc01.telecore.ad.
22.2.5.10.in-addr.arpa. 3600    IN      PTR     SQL-Srv.telecore.ad.
8.2.5.10.in-addr.arpa.  3600    IN      PTR     PKI-Srv.telecore.ad.
2.5.10.in-addr.arpa.    3600    IN      SOA     dc01.telecore.ad. hostmaster.telecore.ad. 6 900 600 86400 3600
;; Query time: 316 msec
;; SERVER: 10.5.2.99#53(10.5.2.99) (TCP)
;; WHEN: Thu Nov 20 19:24:19 EST 2025
;; XFR size: 8 records (messages 1, bytes 304)

```


# Zonas de Transferencia DNS 

## 1. ¿Qué es una zona DNS?

Una "zona" es un segmento de la base de datos DNS que contiene:
- Registros A, AAAA, CNAME, NS, MX, PTR, SRV, SOA.
- Información sobre dominios y subdominios.
- En Active Directory: también registros SRV y zonas integradas.

Cada zona es administrada por un servidor DNS "autoridad" (SOA).

---

## 2. ¿Qué es una transferencia de zona?

Es el proceso por el cual un servidor DNS envía una copia parcial o total de una zona a otro servidor DNS autorizado.

Propósito principal:
- Mantener sincronizados servidores DNS primarios y secundarios.

Se envía por **TCP 53**, no por UDP.

---

## 3. Tipos de transferencias

### AXFR (Complete Zone Transfer)
- Envía **toda** la zona completa.
- Incluye SOA, NS, A, SRV, PTR, MX, CNAME, etc.
- Utilizada cuando un secundario se inicializa o requiere copia total.

### IXFR (Incremental Zone Transfer)
- Envía solo los **cambios** desde la última sincronización.
- Más eficiente.
- Requiere que el servidor tenga almacenadas las diferencias.

---

## 4. ¿Cómo funciona una AXFR?

1. Cliente abre una conexión TCP al puerto 53 del servidor DNS autoritativo.
2. Solicita `TYPE AXFR` para la zona.
3. El servidor:
   - Comprueba si está permitido.
   - Si sí → envía:
     - SOA (inicio)
     - Todos los registros de la zona
     - SOA (fin)
4. El cliente recibe copia exacta de la base de datos DNS.

Ejemplo conceptual de paquete:
