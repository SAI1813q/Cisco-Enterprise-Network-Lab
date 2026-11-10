# Routing and IPv6 Configuration Reference

This guide summarizes the routed design shown in the lab screenshots. The network uses OSPF process 1 in Area 0, connected and static routes, a floating static default route, and IPv6 addressing/static routing.

## Layer 3 VLAN interfaces and routed links

The distribution switch evidence shows routed SVIs for user and management VLANs, loopback interfaces for router IDs, and routed point-to-point links. One Office A distribution screenshot shows these interface addresses:

| Interface | Address shown |
|---|---|
| Loopback0 | 10.0.0.79 |
| Vlan10 | 10.1.0.2 |
| Vlan20 | 10.2.0.2 |
| Vlan40 | 10.6.0.3 |
| Vlan99 | 10.0.0.2 |
| Gi1/1/1 | 10.0.0.46 |
| Gi1/1/2 | 10.0.0.62 |

The exact interface addressing should be taken from the relevant device configuration in the completed Packet Tracer file.

## OSPFv2 – Area 0

The screenshots show OSPF process 1 and Area 0 on the distribution switches, core switches and edge router. The visible router IDs include:

- R1: 10.0.0.76
- Core switch 1: 10.0.0.77
- Office A distribution switch 1: 10.0.0.79

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

The screenshot for R1 identifies it as an autonomous-system boundary router and shows external route redistribution information. Configure redistribution only where intended by the lab design.

Verification:

```cisco
show ip protocols
show ip ospf neighbor
show ip route
show ip route ospf
```

Screenshot references: [21 – Office A DS1 OSPF](../ScreenShots/21-Office-A-DS1-OSPF.png), [22 – Office A DS2 OSPF](../ScreenShots/22-Office-A-DS2-OSPF.png), [23 – Office B DS1 OSPF](../ScreenShots/23-Office-B-DS1-OSPF.png), [24 – Office B DS2 OSPF](../ScreenShots/24-Office-B-DS2-OSPF.png), [25 – Core switch 1 OSPF](../ScreenShots/25-Core-Switch-1-OSPF.png), [26 – Core switch 2 OSPF](../ScreenShots/26-Core-Switch-2-OSPF.png), [27 – Router OSPF](../ScreenShots/27-Router-OSPF.png).

## Static and floating static routes

The R1 routing table screenshot shows a primary default route via 203.0.113.1. The floating static route screenshot shows a second default route via 203.0.113.7 with administrative distance 2.

```cisco
ip route 0.0.0.0 0.0.0.0 203.0.113.1
ip route 0.0.0.0 0.0.0.0 203.0.113.7 2
```

The higher administrative distance makes the second route a backup candidate while the preferred route is installed. The screenshots also show OSPF-learned routes in the routing table.

Verification:

```cisco
show ip route
show running-config | include ip route
```

Screenshot references: [28 – R1 Routing Table](../ScreenShots/28-R1-Routing-Table.png), [29 – R1 Floating Static and OSPF Routes](../ScreenShots/29-R1-Floating-Static-and-OSPF-Routes.png).

## IPv6 addressing and routing

The R1 screenshot shows IPv6 interfaces in the 2001:DB8:A::/64 and 2001:DB8:B::/64 networks, and an IPv6 default route via 2001:DB8:A::1.

```cisco
ipv6 unicast-routing
ipv6 route ::/0 2001:DB8:A::1
```

The interface addresses should match the addressing shown in the topology and the IPv6 interface screenshot.

Verification:

```cisco
show ipv6 interface brief
show ipv6 route
```

Screenshot reference: [30 – IPv6 Interfaces and Routes](../ScreenShots/30-IPv6-Interfaces-and-Routes.png).
