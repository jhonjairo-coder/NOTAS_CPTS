
```powershell
C:\>net group

Group Accounts for \\

-------------------------------------------------------------------------------
*$S31000-F4NJL2TSR1PU
*Cloneable Domain Controllers
*Compliance Management
*Delegated Setup
*Discovery Management
*DnsUpdateProxy
*Domain Admins
*Domain Computers
*Domain Controllers
*Domain Guests
*Domain Users
*Enterprise Admins
*Enterprise Key Admins
*Enterprise Read-only Domain Controllers
*ESX Admins
*Exchange Servers
*Exchange Trusted Subsystem
*Exchange Windows Permissions
*ExchangeLegacyInterop
*Group Policy Creator Owners
*Help Desk
*Hygiene Management
*Key Admins
*Managed Availability Servers
*Organization Management
*Protected Users
*Public Folder Management
*Read-only Domain Controllers
*Recipient Management
*Records Management
*Schema Admins
*Security Administrator
*Security Reader
*Server Management
*UM Management
*View-Only Organization Management
The command completed with one or more errors.

``` 

```powershell
C:\>net group "ESX Admins"
Group name     ESX Admins
Comment        

Members

-------------------------------------------------------------------------------
virt-admin               
The command completed successfully.

```

```powershell
C:\>net group "Exchange Servers"
Group name     Exchange Servers
Comment        This group contains all the Exchange servers. This group shouldn't be deleted.

Members

-------------------------------------------------------------------------------
EX-SRV$                  
The command completed successfully.
```