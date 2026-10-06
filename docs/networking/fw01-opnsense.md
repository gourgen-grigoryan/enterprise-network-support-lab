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
- Observed DHCP lease during deployment: 172.21.138.246/20

Note: The WAN address is dynamically assigned and may change.

### LAN

- OPNsense interface: hn1
- Hyper-V adapter name: LAN
- Hyper-V switch: HyperV-LAN
- MAC address: 00:15:5D:32:32:04
- IPv4 address: 10.20.0.1/24
- IPv6: Disabled for the initial lab phase

## Lab Host Connectivity

- Hyper-V host LAN address: 10.20.0.254/24
- Default gateway on host-side HyperV-LAN adapter: None

## Services

- Web GUI: HTTPS
- Web GUI URL: https://10.20.0.1
- SSH: Enabled for lab administration
- DHCP server on FW01: Disabled

DHCP will later be provided by DC01.

## System Identity

- Hostname: FW01
- Domain: corp.lab.node
- FQDN: FW01.corp.lab.node
- Time zone: Asia/Yerevan

## Verification

The following checks were successfully completed:

- WAN received a DHCP lease
- FW01 reached 8.8.8.8 with 0% packet loss
- DNS resolution succeeded using drill google.com
- HTTPS Web GUI was reachable from the Hyper-V host
- SSH access from MobaXterm was successful
- hostname returned FW01.corp.lab.node
- system time used UTC+04 / Asia/Yerevan

## Security Notes

- No passwords or private credentials are stored in this repository.
- Root password is intentionally excluded from documentation.
- Password-based SSH access is enabled only for the current training phase.
- SSH authentication will be hardened in a later security exercise.
