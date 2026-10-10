# Network Topology

## Current Multi-Host Architecture

The lab is distributed across two Windows 11 Pro Hyper-V hosts connected through the home Wi-Fi network.

```text
                              Internet
                                 |
                         Home Wi-Fi network
                          192.168.50.0/24
                                 |
             +-------------------+-------------------+
             |                                       |
     ROG / PHANTOM                            VivoBook / HOME
     Wi-Fi 192.168.50.50                      Wi-Fi 192.168.50.70
             |                                       |
     IPv4 forwarding                         IPv4 forwarding
             |                                       |
 vEthernet (HyperV-LAN)                  vEthernet (Branch-LAN)
       10.20.0.254                              10.30.0.254
             |                                       |
      HyperV-LAN                                Branch-LAN
      10.20.0.0/24                             10.30.0.0/24
        /        \                                    |
       /          \                                   |
 FW01 LAN        DC01                              WIN11-01 LAB
 10.20.0.1     10.20.0.10                         10.30.0.100
 OPNsense      AD/DNS/DHCP                        Domain client
```

FW01 also has a WAN adapter connected to the ROG Hyper-V Default Switch.

WIN11-01 also has an INTERNET adapter connected to the VivoBook Hyper-V Default Switch.

## Logical Routing Path

Traffic between WIN11-01 and the HQ lab follows this path:

```text
WIN11-01 10.30.0.100
    |
    v
VivoBook Branch-LAN 10.30.0.254
    |
    v
VivoBook Wi-Fi 192.168.50.70
    |
    v
ROG Wi-Fi 192.168.50.50
    |
    v
ROG HyperV-LAN 10.20.0.254
    |
    +--> DC01 10.20.0.10
    |
    +--> FW01 10.20.0.1
```

The Wi-Fi network acts as the routed transport between the two Hyper-V hosts.

There is no NAT between 10.20.0.0/24 and 10.30.0.0/24, and no site-to-site VPN is configured at this stage.

## Systems

- FW01 - OPNsense firewall and edge router
- DC01 - Windows Server 2022 domain controller, DNS, and DHCP
- WIN11-01 - Windows 11 Pro domain workstation
- ROG / PHANTOM - HQ Hyper-V host and software router
- VivoBook / HOME - Branch Hyper-V host and software router
- LNX01 - Planned Ubuntu Linux server
- MON01 - Planned monitoring server
- DB01 - Planned database server

## Verified Connectivity

The following paths have been verified successfully:

- ROG 192.168.50.50 to VivoBook 192.168.50.70
- ROG to WIN11-01 10.30.0.100
- VivoBook to ROG 192.168.50.50
- VivoBook Branch-LAN 10.30.0.254 to WIN11-01
- DC01 10.20.0.10 to WIN11-01 10.30.0.100
- WIN11-01 to DC01 10.20.0.10
- WIN11-01 to FW01 10.20.0.1
- FW01 10.20.0.1 to WIN11-01 10.30.0.100
- WIN11-01 to the Internet through its dedicated INTERNET adapter
- Active Directory DNS resolution and LDAP connectivity from WIN11-01
- WIN11-01 domain secure channel to corp.lab.node

A verified traceroute from DC01 to WIN11-01 follows:

```text
10.20.0.254
192.168.50.70
10.30.0.100
```

A verified traceroute from WIN11-01 to the HQ network follows:

```text
10.30.0.254
192.168.50.50
10.20.0.x
```

## Planned Expansion

- Linux server deployment
- Zabbix monitoring
- PostgreSQL / MariaDB
- VLAN segmentation
- VPN services
- DHCP relay or a dedicated Branch DHCP design
- Backup and recovery
- Troubleshooting scenarios
