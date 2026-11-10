# Cisco Enterprise Network Lab

A multi-office enterprise network designed and implemented in **Cisco Packet Tracer** using a hierarchical three-tier architecture. The lab brings together switching, routing, redundancy, network services, infrastructure security and centrally managed wireless connectivity.

The repository includes both the initial baseline topology and the completed Packet Tracer implementation, along with screenshots documenting the configuration and verification process.

## Project at a glance

- **Platform:** Cisco Packet Tracer
- **Design:** Two-office enterprise network with core, distribution and access layers
- **Routing:** OSPF, static routing, floating static routes and IPv6 static routing
- **Resilience:** EtherChannel, Rapid PVST+ and HSRP
- **Security:** ACLs, port security, DHCP snooping, Dynamic ARP Inspection, SSH and NAT/PAT
- **Services:** DHCP, DNS, NTP, Syslog and FTP
- **Wireless:** Wireless LAN Controller (WLC), WLAN/SSID configuration and wireless client connectivity

## Network topology

The topology is organized into three logical layers and connects two office environments through a shared core.

![Overall enterprise network topology](ScreenShots/01-Network-Topology.png)

### Core layer
- Two interconnected Layer 3 core switches provide the central backbone.
- Layer 3 EtherChannel provides bundled Layer 3 connectivity.
- OSPF is used for dynamic route exchange with the distribution layer and edge router.

### Distribution layer
- Redundant distribution switches serve Office A and Office B.
- Inter-VLAN routing provides communication between permitted VLANs.
- HSRP provides a virtual default gateway for gateway redundancy.
- Rapid PVST+ controls Layer 2 forwarding paths and prevents switching loops.

### Access layer
- Access switches connect end devices to the appropriate VLANs.
- Access and trunk links carry traffic according to the network design.
- Port security and other Layer 2 protections are applied where configured.

## Implementation details

### 1. VLANs and switching
Created and assigned VLANs across both offices, configured access and trunk ports, and practiced trunk negotiation and VLAN propagation.

- VLAN creation and access-port assignment
- 802.1Q trunking and native VLAN configuration
- Dynamic Trunking Protocol (DTP)
- VTP Version 2

### 2. EtherChannel, spanning tree and redundancy
Configured bundled links and redundant Layer 2/gateway behavior to improve availability and control forwarding paths.

- PAgP EtherChannel in Office A
- LACP EtherChannel in Office B
- Layer 3 EtherChannel between Layer 3 devices
- Rapid PVST+ and root bridge placement
- HSRPv2, priority and preemption
- STP root and HSRP active-gateway alignment

### 3. Routing and IP connectivity
Implemented Layer 3 connectivity across VLANs, offices and the core network.

- Inter-VLAN routing
- OSPF Area 0 and neighbor relationships
- Static and floating static routes
- Default routing and route advertisement
- IPv4 addressing and subnetting
- IPv6 addressing and static routes

### 4. DHCP and Layer 2 security
Configured address assignment and switch-level protections.

- DHCP pools and address bindings
- DHCP relay using helper addresses, where required
- DHCP snooping
- Dynamic ARP Inspection (DAI)
- Switch port security

### 5. Access control and address translation
Applied traffic filtering and edge address translation.

- Standard and extended ACLs
- Interface-based ACL application
- NAT and PAT

### 6. Device management and infrastructure services
Configured basic management access and common network services.

- SSH for secure remote CLI access
- Centralized Syslog
- NTP time synchronization
- DNS records and client name-resolution testing
- FTP user setup and file transfer

### 7. FTP transfer and boot workflow
The screenshots document the FTP account/file-transfer steps and the device information before and after the demonstrated boot workflow.

### 8. Wireless infrastructure and neighbor discovery
Configured a controller-based wireless setup and checked directly connected neighbor information.

- Wireless LAN Controller setup
- Dynamic interface configuration
- WLAN/SSID configuration
- Wireless client association
- LLDP neighbor discovery

## Verification

The lab uses Cisco IOS show commands and end-device tests to check configuration state and connectivity. Examples include:

| Feature | Verification command |
|---|---|
| VLANs | `show vlan brief` |
| Trunk links | `show interfaces trunk` |
| EtherChannel | `show etherchannel summary` |
| Spanning tree | `show spanning-tree` |
| HSRP | `show standby brief` |
| OSPF neighbors | `show ip ospf neighbor` |
| IPv4 routing table | `show ip route` |
| IPv6 routing table | `show ipv6 route` |
| DHCP bindings | `show ip dhcp binding` |
| DHCP snooping | `show ip dhcp snooping` |
| ARP inspection | `show ip arp inspection` |
| Port security | `show port-security` |
| NAT translations | `show ip nat translations` |
| LLDP neighbors | `show lldp neighbors` |

Connectivity checks include end-device addressing, DNS resolution, SSH access, wireless client connectivity and routed communication between network segments.

## Packet Tracer files

| File | Description |
|---|---|
| [Enterprise-Network-Baseline.pkt](Packet-Tracer/Enterprise-Network-Baseline.pkt) | Initial topology before the completed configuration |
| [Enterprise-Network-Completed.pkt](Packet-Tracer/Enterprise-Network-Completed.pkt) | Completed enterprise network lab |

Open the completed file in Cisco Packet Tracer to explore the topology and configurations. The baseline file is provided to show the starting point.

## Configuration documentation

Detailed configuration references are available in the [Documentation](Documentation/README.md) folder. The guides cover switching and redundancy, routing and IPv6, security controls, network services, wireless configuration and LLDP, with Cisco IOS command examples and links to the corresponding screenshots.

## Screenshot documentation

The screenshots are grouped by implementation area and numbered in workflow order. Expand a section to view its configuration evidence.

<details>
<summary>Network Topology (1 screenshots)</summary>

**Network Topology**

![Network Topology](ScreenShots/01-Network-Topology.png)

</details>

<details>
<summary>VLANs, Trunking and VTP (12 screenshots)</summary>

**Office A VLANs**

![Office A VLANs](ScreenShots/02-Office-A-VLANs.png)

**Office A Distribution Switch 1 VLANs**

![Office A Distribution Switch 1 VLANs](ScreenShots/03-Office-A-Distribution-Switch-1-VLANs.png)

**Office A Distribution Switch 2 VLANs**

![Office A Distribution Switch 2 VLANs](ScreenShots/04-Office-A-Distribution-Switch-2-VLANs.png)

**Office B VLANs**

![Office B VLANs](ScreenShots/05-Office-B-VLANs.png)

**Office B Distribution Switch 1 VLANs**

![Office B Distribution Switch 1 VLANs](ScreenShots/06-Office-B-Distribution-Switch-1-VLANs.png)

**Office B Distribution Switch 2 VLANs**

![Office B Distribution Switch 2 VLANs](ScreenShots/07-Office-B-Distribution-Switch-2-VLANs.png)

**Office A Trunking**

![Office A Trunking](ScreenShots/08-Office-A-Trunking.png)

**Office B Trunking**

![Office B Trunking](ScreenShots/09-Office-B-Trunking.png)

**Office A DTP**

![Office A DTP](ScreenShots/10-Office-A-DTP.png)

**Office B DTP**

![Office B DTP](ScreenShots/11-Office-B-DTP.png)

**Office A VTP**

![Office A VTP](ScreenShots/12-Office-A-VTP.png)

**Office B VTP**

![Office B VTP](ScreenShots/13-Office-B-VTP.png)

</details>

<details>
<summary>EtherChannel, STP and Gateway Redundancy (7 screenshots)</summary>

**Office A PAgP EtherChannel**

![Office A PAgP EtherChannel](ScreenShots/14-Office-A-PAgP-EtherChannel.png)

**Office B LACP EtherChannel**

![Office B LACP EtherChannel](ScreenShots/15-Office-B-LACP-EtherChannel.png)

**Layer 3 EtherChannel**

![Layer 3 EtherChannel](ScreenShots/16-Layer-3-EtherChannel.png)

**Office A DS1 STP FHRP**

![Office A DS1 STP FHRP](ScreenShots/17-Office-A-DS1-STP-FHRP.png)

**Office A DS2 STP FHRP**

![Office A DS2 STP FHRP](ScreenShots/18-Office-A-DS2-STP-FHRP.png)

**Office B DS1 STP FHRP**

![Office B DS1 STP FHRP](ScreenShots/19-Office-B-DS1-STP-FHRP.png)

**Office B DS2 STP FHRP**

![Office B DS2 STP FHRP](ScreenShots/20-Office-B-DS2-STP-FHRP.png)

</details>

<details>
<summary>Routing and IPv6 (10 screenshots)</summary>

**Office A DS1 OSPF**

![Office A DS1 OSPF](ScreenShots/21-Office-A-DS1-OSPF.png)

**Office A DS2 OSPF**

![Office A DS2 OSPF](ScreenShots/22-Office-A-DS2-OSPF.png)

**Office B DS1 OSPF**

![Office B DS1 OSPF](ScreenShots/23-Office-B-DS1-OSPF.png)

**Office B DS2 OSPF**

![Office B DS2 OSPF](ScreenShots/24-Office-B-DS2-OSPF.png)

**Core Switch 1 OSPF**

![Core Switch 1 OSPF](ScreenShots/25-Core-Switch-1-OSPF.png)

**Core Switch 2 OSPF**

![Core Switch 2 OSPF](ScreenShots/26-Core-Switch-2-OSPF.png)

**Router OSPF**

![Router OSPF](ScreenShots/27-Router-OSPF.png)

**R1 Routing Table**

![R1 Routing Table](ScreenShots/28-R1-Routing-Table.png)

**R1 Floating Static and OSPF Routes**

![R1 Floating Static and OSPF Routes](ScreenShots/29-R1-Floating-Static-and-OSPF-Routes.png)

**IPv6 Interfaces and Routes**

![IPv6 Interfaces and Routes](ScreenShots/30-IPv6-Interfaces-and-Routes.png)

</details>

<details>
<summary>DHCP and Layer 2 Security (9 screenshots)</summary>

**DHCP Pool Configuration**

![DHCP Pool Configuration](ScreenShots/31-DHCP-Pool-Configuration.png)

**DHCP Bindings**

![DHCP Bindings](ScreenShots/32-DHCP-Bindings.png)

**DHCP Snooping**

![DHCP Snooping](ScreenShots/33-DHCP-Snooping.png)

**Office A Dynamic ARP Inspection**

![Office A Dynamic ARP Inspection](ScreenShots/34-Office-A-Dynamic-ARP-Inspection.png)

**Office A ARP Inspection and DHCP Snooping Config**

![Office A ARP Inspection and DHCP Snooping Config](ScreenShots/35-Office-A-ARP-Inspection-and-DHCP-Snooping-Config.png)

**Office B Dynamic ARP Inspection**

![Office B Dynamic ARP Inspection](ScreenShots/36-Office-B-Dynamic-ARP-Inspection.png)

**Office B ARP Inspection and DHCP Snooping Config**

![Office B ARP Inspection and DHCP Snooping Config](ScreenShots/37-Office-B-ARP-Inspection-and-DHCP-Snooping-Config.png)

**Office A Port Security**

![Office A Port Security](ScreenShots/38-Office-A-Port-Security.png)

**Office B Port Security**

![Office B Port Security](ScreenShots/39-Office-B-Port-Security.png)

</details>

<details>
<summary>ACLs and NAT/PAT (4 screenshots)</summary>

**Standard and Extended ACLs**

![Standard and Extended ACLs](ScreenShots/40-Standard-and-Extended-ACLs.png)

**Office A ACLs**

![Office A ACLs](ScreenShots/41-Office-A-ACLs.png)

**R1 ACLs**

![R1 ACLs](ScreenShots/42-R1-ACLs.png)

**NAT and PAT**

![NAT and PAT](ScreenShots/43-NAT-and-PAT.png)

</details>

<details>
<summary>Device Management and Network Services (10 screenshots)</summary>

**SSH Configuration**

![SSH Configuration](ScreenShots/44-SSH-Configuration.png)

**PC1 SSH Access**

![PC1 SSH Access](ScreenShots/45-PC1-SSH-Access.png)

**PC2 SSH Access**

![PC2 SSH Access](ScreenShots/46-PC2-SSH-Access.png)

**R1 Syslog Configuration**

![R1 Syslog Configuration](ScreenShots/47-R1-Syslog-Configuration.png)

**Syslog Server**

![Syslog Server](ScreenShots/48-Syslog-Server.png)

**NTP Master and Client**

![NTP Master and Client](ScreenShots/49-NTP-Master-and-Client.png)

**NTP Client**

![NTP Client](ScreenShots/50-NTP-Client.png)

**DNS Server**

![DNS Server](ScreenShots/51-DNS-Server.png)

**DNS Mapping**

![DNS Mapping](ScreenShots/52-DNS-Mapping.png)

**PC1 DNS Resolution Test**

![PC1 DNS Resolution Test](ScreenShots/53-PC1-DNS-Resolution-Test.png)

</details>

<details>
<summary>FTP Transfer and Boot Workflow (4 screenshots)</summary>

**FTP User Credentials**

![FTP User Credentials](ScreenShots/54-FTP-User-Credentials.png)

**FTP File Transfer**

![FTP File Transfer](ScreenShots/55-FTP-File-Transfer.png)

**FTP Boot Before System Info**

![FTP Boot Before System Info](ScreenShots/56-FTP-Boot-Before-System-Info.png)

**FTP Boot After System Info**

![FTP Boot After System Info](ScreenShots/57-FTP-Boot-After-System-Info.png)

</details>

<details>
<summary>Wireless and Neighbor Discovery (6 screenshots)</summary>

**Wireless LAN Controller**

![Wireless LAN Controller](ScreenShots/58-Wireless-LAN-Controller.png)

**WLC Interface Configuration**

![WLC Interface Configuration](ScreenShots/59-WLC-Interface-Configuration.png)

**WLC WLAN Configuration**

![WLC WLAN Configuration](ScreenShots/60-WLC-WLAN-Configuration.png)

**Wireless Client Connection**

![Wireless Client Connection](ScreenShots/61-Wireless-Client-Connection.png)

**Additional PC1 Configuration**

![Additional PC1 Configuration](ScreenShots/62-Additional-PC1-Configuration.png)

**LLDP Neighbor Discovery**

![LLDP Neighbor Discovery](ScreenShots/63-LLDP-Neighbor-Discovery.png)

</details>

## Tools used

- Cisco Packet Tracer
- Cisco IOS CLI
- Git and GitHub

## Learning outcomes

This project provided hands-on practice in designing a multi-office network, segmenting users with VLANs, configuring Layer 2 and Layer 3 connectivity, implementing routing and redundancy, applying network security controls, deploying common infrastructure services, and validating behavior through CLI output and end-device testing.

## Project status

**Implementation and screenshot documentation uploaded.** The Packet Tracer files and screenshots are included in this repository.

---

**Author:** Sai Teja
