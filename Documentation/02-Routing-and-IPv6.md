# Routing and IPv6 Configuration Reference

This guide summarizes the routed design shown in the completed Packet Tracer lab. It covers R1 interfaces, OSPF Area 0, static and floating routes, and IPv6 addressing and routing.

## R1 interface addressing

R1 is the edge router. The uploaded `show ip interface brief` screenshot shows the following interfaces and addresses. R1 does **not** have separate SVIs for VLANs 10, 20, 30, 40 or 99; those VLAN gateway interfaces are implemented on the distribution switches.

| R1 interface | IPv4 address | Address method | Status |
|---|---|---|---|
| GigabitEthernet0/0 | 10.0.0.33 | NVRAM | up/up |
| GigabitEthernet0/1 | 10.0.0.37 | NVRAM | up/up |
| GigabitEthernet0/2 | Unassigned | — | administratively down/down |
| GigabitEthernet0/0/0 | 203.0.113.2 | DHCP | up/up |
| GigabitEthernet0/1/0 | 203.0.113.6 | DHCP | up/up |
| Loopback0 | 10.0.0.76 | Manual | up/up |
| Vlan1 | Unassigned | — | administratively down/down |

The two routed links toward the internal network use 10.0.0.33 and 10.0.0.37. The two outside-facing interfaces receive 203.0.113.2 and 203.0.113.6 through DHCP. Loopback0 provides R1's router ID address.

Verification:

```cisco
show ip interface brief
show interfaces description
```

Evidence: [R1 interface summary](../ScreenShots/28-R1-Routing-Table.png) and [R1 interface output supplied for this documentation update](../ScreenShots/).

## Layer 3 distribution interfaces

The distribution switches—not R1—provide the VLAN gateway SVIs. The Office A distribution screenshot shows these addresses on one device:

| Interface | Address shown |
|---|---|
| Loopback0 | 10.0.0.79 |
| Vlan10 | 10.1.0.2 |
| Vlan20 | 10.2.0.2 |
| Vlan40 | 10.6.0.3 |
| Vlan99 | 10.0.0.2 |
| Gi1/1/1 | 10.0.0.46 |
| Gi1/1/2 | 10.0.0.62 |

Office B VLAN 30 is the SRV1 server network. The VLAN gateway interfaces for each office are configured on that office's distribution layer.

## OSPFv2 – Area 0

The screenshots show OSPF process 1 and Area 0 on distribution switches, core switches and R1. Visible router IDs include R1 10.0.0.76, Core switch 1 10.0.0.77 and Office A DSW-A1 10.0.0.79.

The Office A distribution screenshot shows these OSPF network statements:

```cisco
router ospf 1
 router-id 10.0.0.79
 network 10.0.0.46 0.0.0.0 area 0
 network 10.0.0.62 0.0.0.0 area 0
 network 10.0.0.79 0.0.0.0 area 0
 network 10.1.0.2 0.0.0.0 area 0
 network 10.2.0.2 0.0.0.0 area 0
 network 10.6.0.3 0.0.0.0 area 0
 network 10.0.0.2 0.0.0.0 area 0
 passive-interface loopback0
 passive-interface vlan10
 passive-interface vlan20
 passive-interface vlan40
```

R1's OSPF screenshot identifies it as an autonomous-system boundary router and shows external route redistribution information. Redistribution is part of the lab's routing design.

Verification:

```cisco
show ip protocols
show ip ospf neighbor
show ip route
show ip route ospf
```

Screenshot references: [21 – Office A DS1 OSPF](../ScreenShots/21-Office-A-DS1-OSPF.png), [22 – Office A DS2 OSPF](../ScreenShots/22-Office-A-DS2-OSPF.png), [23 – Office B DS1 OSPF](../ScreenShots/23-Office-B-DS1-OSPF.png), [24 – Office B DS2 OSPF](../ScreenShots/24-Office-B-DS2-OSPF.png), [25 – Core switch 1 OSPF](../ScreenShots/25-Core-Switch-1-OSPF.png), [26 – Core switch 2 OSPF](../ScreenShots/26-Core-Switch-2-OSPF.png), [27 – Router OSPF](../ScreenShots/27-Router-OSPF.png).

## Static and floating static routes

The R1 routing evidence shows a primary default route via 203.0.113.1 and a floating default route via 203.0.113.7 with administrative distance 2.

```cisco
ip route 0.0.0.0 0.0.0.0 203.0.113.1
ip route 0.0.0.0 0.0.0.0 203.0.113.7 2
```

The higher administrative distance makes the second route a backup candidate while the preferred route is installed. OSPF-learned routes are also visible in the routing table.

Verification:

```cisco
show ip route
show running-config | include ip route
```

Screenshot references: [28 – R1 Routing Table](../ScreenShots/28-R1-Routing-Table.png), [29 – R1 Floating Static and OSPF Routes](../ScreenShots/29-R1-Floating-Static-and-OSPF-Routes.png).

## IPv6 addressing: EUI-64, manual global addresses and link-local addresses

The IPv6 interface output shows both EUI-64-generated global unicast addresses and manually assigned global IPv6 addresses. It also displays link-local addresses on the active interfaces.

| R1 interface | Global IPv6 address shown | Link-local address shown | Addressing |
|---|---|---|---|
| GigabitEthernet0/0 | 2001:DB8:A1:0:20C:CFFF:FE08:5001/64 | FE80::20C:CFFF:FE08:5001 | EUI-64 global address |
| GigabitEthernet0/1 | 2001:DB8:A2:0:20C:CFFF:FE08:5002/64 | FE80::20C:CFFF:FE08:5002 | EUI-64 global address |
| GigabitEthernet0/0/0 | 2001:DB8:A::2/64 | FE80::2E0:A3FF:FE95:4D4E | Manually assigned global address |
| GigabitEthernet0/1/0 | 2001:DB8:B::2/64 | FE80::2E0:F7FF:FE68:6426 | Manually assigned global address |

The EUI-64 interfaces use a /64 prefix and derive the interface identifier from the interface MAC address. The other two interfaces use manually assigned global addresses. Link-local addresses are shown separately and are used for communication on the local link and IPv6 neighbor/router functions.

Representative IOS command patterns:

```cisco
ipv6 unicast-routing

interface gigabitEthernet0/0
 ipv6 address 2001:DB8:A1:0::/64 eui-64
 ipv6 address FE80::20C:CFFF:FE08:5001 link-local

interface gigabitEthernet0/1
 ipv6 address 2001:DB8:A2:0::/64 eui-64
 ipv6 address FE80::20C:CFFF:FE08:5002 link-local

interface gigabitEthernet0/0/0
 ipv6 address 2001:DB8:A::2/64
 ipv6 address FE80::2E0:A3FF:FE95:4D4E link-local

interface gigabitEthernet0/1/0
 ipv6 address 2001:DB8:B::2/64
 ipv6 address FE80::2E0:F7FF:FE68:6426 link-local

ipv6 route ::/0 2001:DB8:A::1
```

These are documentation examples of the address types and prefixes shown in the screenshot. The exact interface commands should be taken from the saved configuration when reproducing the lab.

Verification:

```cisco
show ipv6 interface brief
show ipv6 route
show running-config | section interface
```

Screenshot reference: [30 – IPv6 Interfaces and Routes](../ScreenShots/30-IPv6-Interfaces-and-Routes.png).
