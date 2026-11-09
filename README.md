# Cisco Enterprise Network Infrastructure | Three-Tier Architecture

## 📌 Project Overview

Designed and implemented a multi-office enterprise network using Cisco Packet Tracer, following the **Three-Tier Hierarchical Network Architecture** (Core, Distribution and Access).

The project focuses on building a scalable, redundant and secure enterprise network connecting two office locations through a centralized core infrastructure.

It covers enterprise switching, dynamic routing, gateway redundancy, network security, infrastructure services, IPv4/IPv6 connectivity and centralized wireless management.

## 🏗️ Network Architecture

The network is structured around three hierarchical layers:

<img width="1108" height="611" alt="Screenshot 2026-11-09 115144" src="https://github.com/user-attachments/assets/bb12d1c5-89de-4156-ade5-fd659ca674df" />


### 1. Core Layer
- Two interconnected Layer 3 core switches.
- High-speed backbone connectivity.
- Layer 3 EtherChannel.
- OSPF routing.
- Connectivity to the edge router and Internet.

### 2. Distribution Layer
- Redundant distribution switches across Office A and Office B.
- Inter-VLAN routing.
- HSRPv2 gateway redundancy.
- Rapid PVST+ implementation.
- STP root bridge election and HSRP active gateway alignment.
- Layer 2 and Layer 3 EtherChannel.
- VLAN trunking and routing policies.

### 3. Access Layer
- Multiple access switches across both offices.
- End-device connectivity.
- VLAN segmentation.
- Port security and Layer 2 security mechanisms.
- Connectivity for PCs, IP phones and wireless access points.

### Additional Infrastructure
- Edge router and Internet connectivity.
- Wireless LAN Controller (WLC).
- Lightweight Wireless Access Points.
- Enterprise server infrastructure.
- IP phones and wireless clients.

## ⚙️ Technologies & Implementations

### 1. Switching and VLAN Management
- VLAN creation and assignment.
- VTP Version 2 configuration.
- Access and trunk port configuration.
- Native VLAN configuration.
- Dynamic Trunking Protocol (DTP) management.
- PAgP and LACP EtherChannel.
- Layer 2 and Layer 3 EtherChannel.

### 2. Spanning Tree Protocol & High Availability
- Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+).
- STP root primary and secondary configuration.
- VLAN-based root bridge election.
- HSRPv2 gateway redundancy.
- HSRP priority and preemption.
- STP root bridge and HSRP active gateway alignment.
- Redundant Layer 2 paths and loop prevention.

### 3. Routing and Layer 3 Connectivity
- Inter-VLAN routing.
- OSPF configuration (Area 0).
- OSPF router ID configuration.
- OSPF neighbor relationships.
- Static routing.
- Floating static routes.
- Default route configuration and advertisement.
- IPv4 addressing and subnetting.

### 4. Network Services
- DHCP server configuration.
- DHCP relay using IP helper-address.
- DNS configuration.
- NTP synchronization.
- SNMP configuration.
- Centralized Syslog.
- FTP-based IOS image transfer.
- SSH remote device administration.

### 5. Network Security
- Extended Access Control Lists (ACLs).
- Switch Port Security.
- DHCP Snooping.
- Dynamic ARP Inspection (DAI).
- Secure remote administration using SSH.
- NAT and PAT configuration.

### 6. IPv6 Implementation
- IPv6 addressing.
- IPv6 routing.
- Static IPv6 routes.
- IPv6 default route configuration.
- Recursive and fully specified static routes.

### 7. Wireless Infrastructure
- Wireless LAN Controller configuration.
- Lightweight AP registration.
- Dynamic interface configuration.
- WLAN and SSID creation.
- WPA2-AES wireless security.
- Wireless client connectivity.

## 🧪 Verification and Troubleshooting

The following Cisco IOS commands are used to verify network configurations and operational status:

| Area | Verification Commands |
|---|---|
| VLANs | `show vlan brief` |
| Trunking | `show interfaces trunk` |
| EtherChannel | `show etherchannel summary` |
| Rapid PVST+ | `show spanning-tree` |
| HSRP | `show standby brief` |
| OSPF | `show ip ospf neighbor` |
| Routing | `show ip route` |
| DHCP | `show ip dhcp binding` |
| NAT/PAT | `show ip nat translations` |
| Port Security | `show port-security` |
| DHCP Snooping | `show ip dhcp snooping` |
| DAI | `show ip arp inspection` |
| IPv6 | `show ipv6 route` |

Connectivity testing includes ping tests, routing verification, gateway redundancy checks and end-device communication across VLANs and office locations.

## 🛠️ Tools Used

- Cisco Packet Tracer
- Cisco IOS Command Line Interface (CLI)
- Git
- GitHub

## 📂 Repository Structure

```text
Cisco-Enterprise-Network-Infrastructure/
│
├── README.md
│
├── Packet-Tracer/
│   └── Enterprise-Network.pkt
│
├── Screenshots/
│   ├── Network-Topology.png
│   ├── VLAN-Configuration.png
│   ├── EtherChannel-Configuration.png
│   ├── Rapid-PVST-Configuration.png
│   ├── HSRP-Verification.png
│   ├── OSPF-Configuration.png
│   ├── Network-Services.png
│   ├── Network-Security.png
│   └── Wireless-Configuration.png
│
├── Configurations/
│   ├── Core-Switches.txt
│   ├── Distribution-Switches.txt
│   ├── Access-Switches.txt
│   └── Router.txt
│
└── Documentation/
    └── Network-Design.md
```

## 🎯 Learning Outcomes

Through this implementation, I gained practical experience in:

- Designing enterprise networks using three-tier architecture.
- Configuring VLANs, trunking and EtherChannel.
- Implementing Rapid PVST+ and preventing Layer 2 loops.
- Aligning STP root bridges with HSRP active gateways.
- Configuring dynamic routing using OSPF.
- Implementing gateway redundancy and failover mechanisms.
- Deploying enterprise network services.
- Applying Layer 2 and Layer 3 security controls.
- Configuring IPv4 and IPv6 connectivity.
- Managing enterprise wireless infrastructure.
- Verifying configurations and troubleshooting network connectivity.

## 📌 Project Status

Completed – Cisco Packet Tracer enterprise network implementation.


**Author:** Sai Teja


