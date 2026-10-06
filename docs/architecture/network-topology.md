# Network Topology

## Initial Lab Architecture

                         Internet
                            |
                    Hyper-V Default Switch
                            |
                         FW01 WAN
                    +----------------+
                    |      FW01      |
                    | Firewall/Router|
                    +----------------+
                         FW01 LAN
                         10.20.0.1
                            |
                       HyperV-LAN
                       10.20.0.0/24
                            |
        +-------------------+-------------------+
        |                   |                   |
      DC01               WIN11-01             LNX01
  10.20.0.10               DHCP            10.20.0.20
 Windows Server 2022    Windows 11        Ubuntu Server
 AD / DNS / DHCP

## Initial Systems

- FW01 - Firewall, routing, NAT
- DC01 - Active Directory, DNS, DHCP
- WIN11-01 - Windows domain workstation
- LNX01 - Ubuntu Linux server

## Planned Expansion

- Zabbix monitoring
- PostgreSQL / MariaDB
- VLAN segmentation
- VPN services
- Backup and recovery
- Troubleshooting scenarios
