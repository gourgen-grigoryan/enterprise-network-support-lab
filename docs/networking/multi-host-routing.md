# Multi-Host Routing

## Purpose

The Enterprise Network Support Lab is split across two Hyper-V hosts while preserving separate HQ and Branch subnets.

The physical home Wi-Fi network is used only as the routed underlay between the hosts.

No Layer 2 bridge, Internet Connection Sharing, NAT between lab subnets, or site-to-site VPN is used for HQ-to-Branch traffic.

## Networks

| Network | Purpose |
| --- | --- |
| 192.168.50.0/24 | Physical Wi-Fi transport |
| 10.20.0.0/24 | HQ lab |
| 10.30.0.0/24 | Branch lab |

## Host Interfaces

### ROG / PHANTOM

- Wi-Fi: 192.168.50.50/24
- vEthernet (HyperV-LAN): 10.20.0.254/24
- IPv4 forwarding: Enabled on both interfaces

### VivoBook / HOME

- Wi-Fi: 192.168.50.70/24
- vEthernet (Branch-LAN): 10.30.0.254/24
- IPv4 forwarding: Enabled on both interfaces

Both Wi-Fi addresses are reserved on the home router.

The former VivoBook address `192.168.50.63` is no longer part of the active topology.

## Static Routes

### ROG

```text
10.30.0.0/24 -> 192.168.50.70 via Wi-Fi
```

### VivoBook

```text
10.20.0.0/24 -> 192.168.50.50 via Wi-Fi
```

### DC01

```text
10.30.0.0/24 -> 10.20.0.254 via Ethernet
```

### FW01

```text
10.30.0.0/24 -> ROG_LAB_GW 10.20.0.254
```

### WIN11-01

```text
10.20.0.0/24 -> 10.30.0.254 via LAB
192.168.50.50/32 -> 10.30.0.254 via LAB
```

The WIN11-01 host route to 192.168.50.50 exists so replies to the ROG management/diagnostic address use the LAB path instead of the separate INTERNET default route.

## WIN11-01 Split Connectivity

WIN11-01 is dual-homed:

- LAB - static 10.30.0.100/24, DNS 10.20.0.10, no default gateway
- INTERNET - Hyper-V Default Switch, DHCP, owns the default route

This keeps Active Directory and lab traffic on LAB while preserving direct Internet access through the Hyper-V Default Switch.

## Diagnostic Firewall Rules

Narrow inbound ICMP rules are used only where required for routing verification.

### ROG

- Allow ICMPv4 Echo from VivoBook 192.168.50.70 on Wi-Fi / Public
- Allow ICMPv4 Echo from WIN11-01 10.30.0.100 on Wi-Fi / Public

### VivoBook

- Allow ICMPv4 Echo from ROG 192.168.50.50 on Wi-Fi / Public
- Allow ICMPv4 Echo from WIN11-01 10.30.0.100 on vEthernet (Branch-LAN)

### WIN11-01

Inbound ICMPv4 Echo is allowed on LAB / Domain from:

- VivoBook Branch-LAN address 10.30.0.254
- ROG transport address 192.168.50.50
- FW01 10.20.0.1
- DC01 10.20.0.10

### FW01

A narrow OPNsense rule allows ICMP Echo Request from BRANCH_NET 10.30.0.0/24 to FW01_LAN_ADDR 10.20.0.1.

## Verified End-to-End Paths

### WIN11-01 to FW01

```text
10.30.0.100
10.30.0.254
192.168.50.50
10.20.0.1
```

### WIN11-01 to DC01

```text
10.30.0.100
10.30.0.254
192.168.50.50
10.20.0.10
```

### DC01 to WIN11-01

```text
10.20.0.10
10.20.0.254
192.168.50.70
10.30.0.100
```

### FW01 to WIN11-01

FW01 diagnostics ping with source 10.20.0.1 to 10.30.0.100 completed with 0% packet loss.

## Design Notes

- 192.168.50.0/24 is only the transport underlay.
- 10.20.0.0/24 and 10.30.0.0/24 remain distinct routed lab networks.
- There is no NAT between the HQ and Branch networks.
- A real site-to-site VPN can replace the current underlay routing in a later lab phase.
- IPv4 forwarding persistence should be re-verified after future host rebuilds or major network changes.
