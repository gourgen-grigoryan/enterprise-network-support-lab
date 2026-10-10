# WIN11-01 - Windows Domain Client

## Purpose

WIN11-01 is the Windows 11 domain workstation used to validate client-side Active Directory, DNS, routing, firewall, and troubleshooting scenarios.

It runs on the VivoBook Hyper-V host on the Branch-LAN subnet.

## Operating System

- Operating system: Windows 11 Pro
- Hypervisor: Microsoft Hyper-V
- VM generation: Generation 2
- Secure Boot: Enabled
- Virtual TPM: Enabled
- Domain: corp.lab.node
- Computer name: WIN11-01

## Network Interfaces

WIN11-01 is dual-homed.

### LAB

- Hyper-V switch: Branch-LAN
- IPv4 address: 10.30.0.100/24
- Default gateway: None
- DNS server: 10.20.0.10
- DNS registration: Enabled
- Network profile: DomainAuthenticated

Static routes:

```text
10.20.0.0/24 -> 10.30.0.254
192.168.50.50/32 -> 10.30.0.254
```

### INTERNET

- Hyper-V switch: Default Switch
- IPv4 configuration: DHCP
- Network profile: Public
- DNS registration: Disabled

An observed DHCP lease during verification was:

- IPv4 address: 172.28.217.86/20
- Default gateway: 172.28.208.1
- DNS server: 172.28.208.1
- Connection-specific suffix: mshome.net

These values are dynamic and may change.

The INTERNET adapter owns the default IPv4 route. LAB has no default gateway.

## Active Directory Integration

WIN11-01 was successfully joined to corp.lab.node.

Verified:

- FQDN: WIN11-01.corp.lab.node
- Domain: corp.lab.node
- Active Directory computer object exists
- Test-ComputerSecureChannel returns True
- LDAP TCP/389 to DC01 succeeds over LAB
- corp.lab.node resolves through DC01 DNS

## Firewall Rules

Narrow inbound ICMP rules are configured on the LAB interface for lab diagnostics.

Allowed sources include:

- VivoBook Branch-LAN address 10.30.0.254
- ROG transport address 192.168.50.50
- FW01 10.20.0.1
- DC01 10.20.0.10

The rules use the Domain profile because LAB is DomainAuthenticated.

## Verified Routing

Route selection to DC01:

```text
Destination: 10.20.0.10
Interface: LAB
Source: 10.30.0.100
Route: 10.20.0.0/24
Next hop: 10.30.0.254
```

Route selection to the Internet:

```text
Destination: Internet
Interface: INTERNET
Default route: Hyper-V Default Switch
```

Route selection to the ROG host:

```text
Destination: 192.168.50.50
Interface: LAB
Route: 192.168.50.50/32
Next hop: 10.30.0.254
```

No LAB route exists for the VivoBook Wi-Fi address 192.168.50.70. WIN11-01 reaches the VivoBook for lab routing through 10.30.0.254 instead.

## Verification

The following tests were completed successfully:

- Ping to VivoBook Branch-LAN gateway 10.30.0.254
- Ping to ROG 192.168.50.50
- Ping to DC01 10.20.0.10
- Ping to FW01 10.20.0.1
- Ping to 8.8.8.8
- DNS resolution for corp.lab.node through 10.20.0.10
- TCP/389 connectivity to dc01.corp.lab.node
- Domain secure channel verification
- Traceroute to FW01 through 10.30.0.254 and 192.168.50.50
- Traceroute to DC01 through 10.30.0.254 and 192.168.50.50

## DHCP Note

WIN11-01 does not currently consume the DC01 CORP-LAN DHCP scope because it resides on a different routed subnet and no DHCP relay is configured.

The Branch-LAN address 10.30.0.100 is therefore static at this stage.
