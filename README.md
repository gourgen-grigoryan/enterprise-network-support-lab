# Enterprise Network Support Lab

Hands-on enterprise network and systems administration lab built with Hyper-V.

This project is used to practice and document:

- Windows Server administration
- Active Directory
- DNS and DHCP
- Group Policy
- Windows client support
- Linux administration
- Networking and troubleshooting
- Firewalls, NAT, routing, and VPN
- Monitoring with Zabbix
- Backup and restore
- PostgreSQL and MariaDB/MySQL
- PowerShell and Bash
- Incident troubleshooting

## Current Status

The core Active Directory, DNS, and DHCP foundation is operational, and the lab now includes a Windows 11 domain workstation on a routed branch subnet hosted by a second Hyper-V computer.

Current verified infrastructure:

- FW01 - OPNsense firewall/router on the HQ lab network
- DC01 - Windows Server 2022 domain controller providing AD DS, DNS, and DHCP
- WIN11-01 - Windows 11 Pro domain workstation on Branch-LAN
- HQ lab network - 10.20.0.0/24
- Branch lab network - 10.30.0.0/24
- Physical Wi-Fi transport - 192.168.50.0/24 between the two Hyper-V hosts

The HQ and Branch lab networks are routed across the existing Wi-Fi transport using Windows IPv4 forwarding and explicit static routes. No site-to-site VPN is configured yet.

WIN11-01 is joined to `corp.lab.node` and has verified connectivity to DC01, FW01, the ROG host, the VivoBook branch gateway, and the Internet.

## Architecture Documentation

- `docs/architecture/network-topology.md`
- `docs/architecture/ip-addressing.md`
- `docs/networking/multi-host-routing.md`
- `docs/networking/fw01-opnsense.md`
- `docs/windows/dc01-ad-dns-dhcp.md`
- `docs/windows/win11-01-domain-client.md`

## Author

Gourgen Grigoryan
