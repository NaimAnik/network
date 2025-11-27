# OSPF Network Design and Implementation 🌐

## Overview
This project details the configuration of a small-to-medium enterprise network featuring **three Cisco routers (R1, R2, R3)** and **two switches (S1, S2)**. The core routing mechanism is **OSPF**, configured to support a **dual-stack environment (IPv4/IPv6)** in a single backbone area. The configuration emphasizes robust network connectivity, subnetting for efficient address space utilization, and foundational device security via SSH.



***

## Objectives
The primary goals of this configuration are:
* ✅ Implement **OSPFv2 (IPv4)** and **OSPFv3 (IPv6)** routing processes in **Area 0** for fast convergence and dynamic path selection.
* ✅ Utilize **Variable Length Subnet Masking (VLSM)** for efficient IP address allocation across point-to-point and LAN links.
* ✅ Configure **DHCP services** on the central router (R1) to automatically assign IPv4 addresses to client devices in the LAN segments.
* ✅ Configure **VLANs** on the switches (S1, S2) for traffic segmentation and management access.
* ✅ Secure all network devices with **SSH access**, local authentication, and password encryption.

***

## Network Details

### 1. Link & IP Addressing

| Link | Device/Interface | IPv4 Address/Subnet | IPv6 Address/Prefix | Area | Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R1-R2 Link** | R1 S0/1/0 | `172.16.10.1 /30` | `2001:db8:1:40::1/64` | 0 | Point-to-Point |
| | R2 S0/1/0 | `172.16.10.2 /30` | `2001:db8:1:40::2/64` | 0 | Point-to-Point |
| **R2-R3 Link** | R2 S0/1/1 | `172.16.10.5 /30` | `2001:db8:1:50::1/64` | 0 | Point-to-Point |
| | R3 S0/1/1 | `172.16.10.6 /30` | `2001:db8:1:50::2/64` | 0 | Point-to-Point |

***

### 2. LAN & VLAN Details

| Device/Interface | VLAN ID | IPv4 Address/Subnet | Network | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **R1 G0/0/0** | **VLAN 10** | **`172.16.10.33 /27`** | `172.16.10.32/27` | N/A (Router SVI) |
| **S1 VLAN 10** | 10 | `172.16.10.34 /27` | `172.16.10.32/27` | `172.16.10.33` (R1) |
| **R3 G0/0/0** | **VLAN 20** | **`172.16.10.65 /27`** | `172.16.10.64/27` | N/A (Router SVI) |
| **S2 VLAN 20** | 20 | `172.16.10.98 /27` | `172.16.10.96/27` | `172.16.10.65` (R3) |

***

## Access and Security Credentials

| Credential | Value | Device Context |
| :--- | :--- | :--- |
| **Enable Secret** | `cisco123` (Encrypted) | Privileged EXEC mode password on all devices. |
| **SSH Username** | `admin` | Used for VTY/SSH login. |
| **SSH Password** | `123` | Used for VTY/SSH login. |
| **VTY / Console** | Local Login | Uses the `admin` / `123` credentials. |
| **Banner MOTD** | `Unauthorized access is prohibited. [Router/Switch]#` | Displays on connect. |
| **Security Features** | `service password-encryption` | All passwords are encrypted in the running-config. |
| | `ip domain-name nti.com` | Configured for SSH/Local Authentication. |

***

## Critical Configuration Review & Fixes

During the configuration review, the following critical issues were identified and **must be implemented** for the network to function correctly:

| Issue | Details | Required Action / Fix |
| :--- | :--- | :--- |
| **DHCP/VLAN 20 Mismatch** | The **R3 LAN (VLAN 20)** gateway (`172.16.10.65`) is in the **`172.16.10.64/27`** subnet, but the R1 DHCP pool is configured for the **`172.16.10.96/27`** subnet. | **Change the DHCP Pool `VLAN20` network statement on R1:** `network 172.16.10.64 255.255.255.224` |
| **Missing DHCP Relay** | Clients in the VLAN 20 segment (connected to R3) will not reach the DHCP server (R1) because R3 is not forwarding the DHCP broadcast. | **Configure a DHCP Helper Address on R3's LAN interface:** `int G0/0/0` -> `ip helper-address 172.16.10.1` |
| **Missing OSPF Authentication** | OSPF neighbor updates are unauthenticated, posing a security risk. | **Implement OSPF MD5 authentication** on all router interfaces participating in OSPF (R1: S0/1/0, G0/0/0; R2: S0/1/0, S0/1/1; R3: S0/1/1, G0/0/0). |

***

## Files
* **Router and Switch Configurations**: [Configuration File.txt](uploaded:Configuration%20File.txt)
* **Cisco Packet Tracer Topology**: [OSPF in Action-Network Build.pkt](uploaded:OSPF%20in%20Action-Network%20Build.pkt)
* **License**: [LICENSE](uploaded:LICENSE)
