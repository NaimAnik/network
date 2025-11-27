# OSPF Network Design and Implementation

## Overview
This project configures OSPF routing between multiple Cisco routers with dual-stack support (IPv4/IPv6) and subnetting. The configuration focuses on OSPF implementation, ensuring optimized routing between various network segments.
![image](https://github.com/user-attachments/assets/cb955442-3475-4870-b721-d6d1194b49fe)


## Topology
- **Subnets**: IPv4 and IPv6 dual-stack setup for efficient routing.
- **Routers**: ISR4321 Routers (Handling OSPF routing between different networks).
- **Switches**: 2960-24TT Switches (Handling VLAN segmentation and trunking).
- **Devices**: PCs configured with static IP addresses for communication within VLANs.

## Objectives:
- Implement OSPF routing for efficient communication between networks.
- Use IPv4 and IPv6 subnetting to create structured network addresses.
- Configure VLANs on switches for better traffic segmentation.
- Secure the routers and switches with password encryption and SSH access.

## Network Details:
### Subnet Configuration:
- **Subnet 1**: `172.16.10.0/24` for the main network.
- **IPv6 Subnet**: `2001:db8:1::/48` addressing for IPv6 network communication.
- **Point-to-Point links between routers**:
  - `172.16.10.0/30` (R1-R2 link)
  - `172.16.10.4/30` (R2-R3 link)

### Addressing Table:
Refer to the [Addressing Table and Objectives](OSPF%20in%20Action-Network%20Build.pdf) for complete IP address assignments, or check the details below:

| **Device**        | **Interface**       | **IP Address**          | **Subnet Mask/Prefix** | **Description**              |
|-------------------|---------------------|------------------------|-----------------------|-----------------------------|
| **Router 1 (R1)** | `G0/0/0`            | `172.16.10.33`         | `255.255.255.224`     | Network to switch S1        |
|                   | `S0/1/0`            | `172.16.10.1`          | `255.255.255.252`     | Link to R2                  |
| **Router 2 (R2)** | `S0/1/0`            | `172.16.10.2`          | `255.255.255.252`     | Link to R1                  |
|                   | `S0/2/0`            | `172.16.10.6`          | `255.255.255.252`     | Link to R3                  |
| **Router 3 (R3)** | `S0/2/0`            | `172.16.10.5`          | `255.255.255.252`     | Link to R2                  |
| **Switch 1 (S1)** | `VLAN 10`           | `172.16.10.34`         | `255.255.255.224`     | Management VLAN             |
| **Switch 2 (S2)** | `VLAN 10`           | `172.16.10.98`         | `255.255.255.224`     | Management VLAN             |

### VLAN Table:
| **VLAN**      | **VLAN ID** | **Subnet**           | **Description**         |
|---------------|-------------|---------------------|-------------------------|
| **VLAN 10**   | 10          | `172.16.10.32/27`   | Network connected to S1 |
| **VLAN 20**   | 20          | `172.16.10.96/27`   | Network connected to S2 |
| **Management VLAN** | 100   | `192.168.100.0/29`  | Secure management access|

## Network Topology
Refer to the [Topology](OSPF%20in%20Action-Network%20Build.pkt) for a visual representation of the lab setup.
- **User**: admin
- **Password**: 123
- **Privilege level**: cisco123

## Configuration Files
- **Router and Switch Configurations**: Find all configurations in the [Configuration File](Configuration%20File.txt).

## Steps to Implement:
1. Configure OSPF routing on routers.
2. Set up VLANs on switches.
3. Secure devices using SSH, password encryption, and banners.
4. Assign appropriate IP addresses to interfaces.
5. Verify connectivity and OSPF routes between devices.

## Contact
For any questions or feedback, please reach out to:
- **Name**: Mohamed Khaled Mahmoud Sayed
- **Email**: Mo7ammad244@gmail.com

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
