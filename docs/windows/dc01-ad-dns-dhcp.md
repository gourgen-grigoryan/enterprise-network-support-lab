# DC01 - Active Directory, DNS, and DHCP

## Purpose

DC01 is the primary Windows infrastructure server for the Enterprise Network Support Lab.

It currently provides:

- Active Directory Domain Services
- DNS
- DHCP
- Group Policy management infrastructure

## Platform

- Hypervisor: Microsoft Hyper-V
- Operating System: Windows Server 2022 Standard Evaluation (Desktop Experience)
- VM Generation: Generation 2
- vCPU: 2
- Memory: 4096 MB
- Virtual Disk: 80 GB dynamically expanding VHDX
- Secure Boot: Enabled
- Hyper-V switch: HyperV-LAN

## Network Configuration

- Hostname: DC01
- FQDN: DC01.corp.lab.node
- IPv4 address: 10.20.0.10/24
- Default gateway: 10.20.0.1
- DNS server: 10.20.0.10
- Time zone: Caucasus Standard Time / UTC+04:00 Yerevan

DC01 uses itself as its DNS resolver after promotion to a domain controller.

## Active Directory

DC01 is the first domain controller in a new Active Directory forest.

- DNS domain: corp.lab.node
- NetBIOS domain: CORP
- Forest root domain: corp.lab.node
- Forest functional level: Windows Server 2016
- Domain functional level: Windows Server 2016
- Global Catalog: Enabled
- DNS Server role: Enabled
- Read-Only Domain Controller: No

Administrative and Directory Services Restore Mode passwords are intentionally excluded from this repository.

## DNS

The DNS Server role was installed as part of the domain controller deployment.

Verified internal DNS resolution:

- corp.lab.node resolves to 10.20.0.10
- _ldap._tcp.dc._msdcs.corp.lab.node returns DC01.corp.lab.node
- DC01.corp.lab.node resolves to the domain controller

External DNS resolution was also verified successfully from DC01.

## DHCP

The DHCP Server role is installed on DC01 and authorized in Active Directory.

### Scope

- Scope name: CORP-LAN
- Scope ID: 10.20.0.0
- Start address: 10.20.0.100
- End address: 10.20.0.199
- Subnet mask: 255.255.255.0
- Lease duration: 8 days
- State: Active

### Scope Options

- Option 003 Router: 10.20.0.1
- Option 006 DNS Servers: 10.20.0.10
- Option 015 DNS Domain Name: corp.lab.node

Infrastructure systems use static addresses outside the DHCP pool.

WIN11-01 will be used to validate DHCP leasing, DNS registration, and Active Directory domain join.

## Verification

The following checks were successfully completed:

- DC01 hostname and domain membership verified
- Active Directory domain information verified with Get-ADDomain
- Internal Active Directory DNS records resolved successfully
- External DNS resolution succeeded
- DHCP Server service is running
- DHCP server is authorized in Active Directory
- CORP-LAN scope is active
- DHCP scope options were verified
- DHCP address pool is currently available for client deployment

## Security Notes

- No passwords or private credentials are stored in this repository.
- Domain Administrator credentials are intentionally excluded.
- Directory Services Restore Mode credentials are intentionally excluded.
- DHCP authorization is integrated with Active Directory.
