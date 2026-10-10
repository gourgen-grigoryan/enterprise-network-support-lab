# FW01 - OPNsense Firewall

## Purpose

FW01 is the edge firewall and router for the Enterprise Network Support Lab.

## Platform

- Hypervisor: Microsoft Hyper-V
- Operating System: OPNsense 26.7
- VM Generation: Generation 2
- vCPU: 1
- Memory: 3072 MB
- Virtual Disk: 20 GB VHDX
- Secure Boot: Disabled

## Network Interfaces

### WAN

- OPNsense interface: hn0
- Hyper-V adapter name: WAN
- Hyper-V switch: Default Switch
- MAC address: 00:15:5D:32:32:03
- IPv4 configuration: DHCP

The WAN address is dynamically assigned by the Hyper-V Default Switch and may change.

### LAN

- OPNsense interface: hn1
- Hyper-V adapter name: LAN
- Hyper-V switch: HyperV-LAN
- MAC address: 00:15:5D:32:32:04
- IPv4 address: 10.20.0.1/24
- IPv6: Disabled for the initial lab phase

## Lab Host Connectivity

- ROG Hyper-V host LAN address: 10.20.0.254/24
- Default gateway on host-side HyperV-LAN adapter: None

## Branch Routing

FW01 has an explicit route to the Branch-LAN network.

### Gateway

- Name: ROG_LAB_GW
- Interface: LAN
- Address family: IPv4
- Gateway IP: 10.20.0.254
- Priority: 255
- Upstream gateway: No
- Gateway monitoring: Disabled
- Description: ROG host route to Branch-LAN

### Static Route

- Destination: 10.30.0.0/24
- Gateway: ROG_LAB_GW - 10.20.0.254
- Description: Route to Branch-LAN via ROG

The active OPNsense routing table was verified to contain:

```text
10.30.0.0/24 -> 10.20.0.254 via hn1 (LAN)
```

FW01 does not route directly to the VivoBook Wi-Fi transport address. The ROG host is the only FW01 next hop for Branch-LAN.

## Branch Diagnostic Firewall Rule

Two aliases are defined:

- BRANCH_NET = 10.30.0.0/24
- FW01_LAN_ADDR = 10.20.0.1

A narrow diagnostic firewall rule allows ICMP Echo Request traffic from Branch-LAN to the FW01 LAN address:

- Interface: LAN
- Direction: In
- IP version: IPv4
- Protocol: ICMP
- ICMP type: Echo Request
- Source: BRANCH_NET
- Destination: FW01_LAN_ADDR
- Gateway: None
- Description: Allow Branch-LAN ICMP to FW01

This rule is intentionally limited to diagnostic ping traffic to FW01 itself.

## Services

- Web GUI: HTTPS
- Web GUI URL: https://10.20.0.1
- SSH: Enabled for lab administration
- DHCP server on FW01: Disabled

DHCP is provided by DC01 at 10.20.0.10.

## System Identity

- Hostname: FW01
- Domain: corp.lab.node
- FQDN: FW01.corp.lab.node
- Time zone: Asia/Yerevan

## Verification

The following checks were successfully completed:

- WAN received a DHCP lease
- Internet connectivity from FW01 was verified
- DNS resolution from FW01 was verified
- HTTPS Web GUI was reachable from the Hyper-V host
- SSH access was verified
- Hostname returned FW01.corp.lab.node
- Static Branch-LAN route was present in the active routing table
- FW01 source 10.20.0.1 successfully pinged WIN11-01 at 10.30.0.100 with 0% loss
- WIN11-01 successfully pinged FW01 at 10.20.0.1 after the narrow Branch ICMP rule was applied

## Security Notes

- No passwords or private credentials are stored in this repository.
- Root password is intentionally excluded from documentation.
- Password-based SSH access is enabled only for the current training phase.
- SSH authentication will be hardened in a later security exercise.
