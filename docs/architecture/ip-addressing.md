# IP Addressing Plan

## Network Roles

The lab currently uses three IPv4 networks:

| Network | Role |
| --- | --- |
| 192.168.50.0/24 | Physical Wi-Fi transport between Hyper-V hosts |
| 10.20.0.0/24 | HQ lab network |
| 10.30.0.0/24 | Branch lab network |

## Physical Wi-Fi Transport

The physical hosts use router-managed DHCP reservations so their transport addresses remain stable.

| System | Interface | Address |
| --- | --- | --- |
| ROG / PHANTOM | Wi-Fi | 192.168.50.50/24 |
| VivoBook / HOME | Wi-Fi | 192.168.50.70/24 |

The former VivoBook address `192.168.50.63` is no longer used.

The Wi-Fi network is an underlay transport only. It is not the Active Directory client subnet.

## HQ Lab Network

- Network: 10.20.0.0/24
- Subnet mask: 255.255.255.0
- Default gateway for HQ guests: 10.20.0.1

### Static Infrastructure

| System | Role | Address |
| --- | --- | --- |
| FW01 | OPNsense firewall/router | 10.20.0.1 |
| DC01 | AD DS / DNS / DHCP | 10.20.0.10 |
| LNX01 | Planned Linux server | 10.20.0.20 |
| MON01 | Planned monitoring server | 10.20.0.30 |
| DB01 | Planned database server | 10.20.0.40 |
| ROG host | HyperV-LAN router interface | 10.20.0.254 |

### DHCP Scope

DC01 provides the CORP-LAN DHCP scope for 10.20.0.0/24:

- Start: 10.20.0.100
- End: 10.20.0.199
- Router option: 10.20.0.1
- DNS option: 10.20.0.10
- DNS domain: corp.lab.node

## Branch Lab Network

- Network: 10.30.0.0/24
- Subnet mask: 255.255.255.0

| System | Interface | Address |
| --- | --- | --- |
| VivoBook host | vEthernet (Branch-LAN) | 10.30.0.254/24 |
| WIN11-01 | LAB | 10.30.0.100/24 |

WIN11-01 currently uses a static Branch-LAN address. DHCP relay from DC01 to Branch-LAN has not been configured.

The WIN11-01 LAB interface has no default gateway. It uses explicit lab routes instead, while Internet access uses a second Hyper-V Default Switch adapter.

## WIN11-01 Internet Adapter

The INTERNET adapter is connected to the VivoBook Hyper-V Default Switch and uses DHCP.

An observed lease during verification was:

- Address: 172.28.217.86/20
- Gateway: 172.28.208.1
- DNS: 172.28.208.1

These values are dynamic and may change.

DNS registration is disabled on the INTERNET adapter so the lab DNS identity is associated with the LAB interface instead.
