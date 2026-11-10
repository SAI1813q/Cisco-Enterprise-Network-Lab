# Switching and Redundancy Configuration Reference

This guide records the switching and first-hop redundancy configuration shown in the completed Packet Tracer lab and its screenshots.

## VLAN segmentation

| VLAN | Purpose | Office |
|---:|---|---|
| 10 | PCs | A and B |
| 20 | Phones | A and B |
| 30 | Server network (SRV1) | B |
| 40 | Wi-Fi | A |
| 99 | Management | A and B |

**Important:** VLAN 30 is the server VLAN for SRV1 in Office B. It is not the Wi-Fi VLAN. VLAN 40 is used for Wi-Fi in Office A.

The VLAN creation commands are shown as a reference pattern; configure only the VLANs required on each switch:

```cisco
configure terminal
vlan 10
 name pcs
vlan 20
 name phones
vlan 30
 name servers
vlan 40
 name wifi
vlan 99
 name management
end
show vlan brief
```

## Access ports and 802.1Q trunking

Access ports assign an endpoint-facing interface to one VLAN. Trunks carry multiple VLANs between network devices. The Office A trunk evidence shows native VLAN 1000 and allowed VLANs 10, 20, 40 and 99.

```cisco
interface <access-interface>
 switchport mode access
 switchport access vlan <vlan-id>

interface <trunk-interface>
 switchport mode trunk
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,40,99
```

Use the Office B allowed VLAN list shown in its own trunk screenshot rather than copying the Office A list.

Verification:

```cisco
show vlan brief
show interfaces trunk
show interfaces <interface-id> switchport
```

Screenshot references: [08 – Office A Trunking](../ScreenShots/08-Office-A-Trunking.png), [09 – Office B Trunking](../ScreenShots/09-Office-B-Trunking.png).

## DTP and VTP

The DTP evidence shows trunk negotiation disabled on the displayed trunk interface (Negotiation of Trunking: Off), indicating a statically configured trunk.

The VTP status evidence shows VTP Version 2, domain `JeremySITLab`, and the displayed switch operating as a server.

```cisco
show interfaces <interface-id> switchport
show vtp status
show vtp counters
```

Screenshot references: [10 – Office A DTP](../ScreenShots/10-Office-A-DTP.png), [11 – Office B DTP](../ScreenShots/11-Office-B-DTP.png), [12 – Office A VTP](../ScreenShots/12-Office-A-VTP.png), [13 – Office B VTP](../ScreenShots/13-Office-B-VTP.png).

## EtherChannel

The lab evidence shows three EtherChannel implementations:

- Office A: PAgP, Port-channel 1, member interfaces GigabitEthernet1/0/4 and GigabitEthernet1/0/5.
- Office B: LACP, Port-channel 1, member interfaces GigabitEthernet1/0/4 and GigabitEthernet1/0/5.
- Core Layer 3 bundle: Port-channel 1, PAgP, with member interfaces GigabitEthernet1/0/2 and GigabitEthernet1/0/3. The displayed Port-channel IP is 10.0.0.42.

Representative configuration patterns:

```cisco
! PAgP
interface range gigabitEthernet1/0/4-5
 channel-group 1 mode desirable

! LACP
interface range gigabitEthernet1/0/4-5
 channel-group 1 mode active
```

The Layer 3 bundle uses routed physical interfaces and a routed Port-channel:

```cisco
interface range gigabitEthernet1/0/2-3
 no switchport
 channel-group 1 mode desirable
interface port-channel1
 no switchport
 ip address 10.0.0.42 255.255.255.252
```

Verification:

```cisco
show etherchannel summary
show interfaces port-channel1
```

The captured Layer 2 bundle is shown as `SU`, the Layer 3 bundle as `RU`, and bundled member interfaces as `(P)`.

Screenshot references: [14 – Office A PAgP](../ScreenShots/14-Office-A-PAgP-EtherChannel.png), [15 – Office B LACP](../ScreenShots/15-Office-B-LACP-EtherChannel.png), [16 – Layer 3 EtherChannel](../ScreenShots/16-Layer-3-EtherChannel.png).

## Rapid PVST+ root bridge and HSRPv2 alignment

Rapid PVST+ and HSRPv2 are configured together so that the distribution switch acting as the STP root for a VLAN is also the HSRP active gateway for that VLAN. This keeps the Layer 2 forwarding root aligned with the first-hop gateway role and avoids unnecessary traffic paths.

The screenshots show the following verified root/active alignment:

| Office | VLAN | VLAN purpose | RPVST+ root | HSRP active |
|---|---:|---|---|---|
| A | 10 | PCs | DSW-A1 | DSW-A1 |
| A | 20 | Phones | DSW-A2 | DSW-A2 |
| A | 40 | Wi-Fi | DSW-A2 | DSW-A2 |
| A | 99 | Management | DSW-A1 | DSW-A1 |
| B | 10 | PCs | DSW-B1 | DSW-B1 |
| B | 20 | Phones | DSW-B2 | DSW-B2 |
| B | 30 | SRV1 server network | DSW-B2 | DSW-B2 |
| B | 99 | Management | DSW-B1 | DSW-B1 |

The evidence shows VLAN-specific STP priorities matching the VLAN IDs on the selected root switches. HSRP priorities are set higher on the same switches, with preemption enabled.

Office A HSRP virtual IPs shown in the output:

| VLAN | HSRP group | Virtual IP |
|---:|---:|---|
| 10 | 2 | 10.1.0.1 |
| 20 | 3 | 10.2.0.1 |
| 40 | 4 | 10.6.0.1 |
| 99 | 1 | 10.0.0.1 |

Office B HSRP virtual IPs shown in the output:

| VLAN | HSRP group | Virtual IP |
|---:|---:|---|
| 10 | 2 | 10.3.0.1 |
| 20 | 3 | 10.4.0.1 |
| 30 | 4 | 10.5.0.1 |
| 99 | 1 | 10.0.0.17 |

Example HSRP configuration pattern (use the exact group, virtual IP and priority for the relevant VLAN/device):

```cisco
interface vlan 10
 standby version 2
 standby 2 ip 10.1.0.1
 standby 2 priority 105
 standby 2 preempt
```

Verification:

```cisco
show standby brief
show spanning-tree vlan <vlan-id>
show spanning-tree
```

Screenshot references: [17 – Office A DS1 STP/FHRP](../ScreenShots/17-Office-A-DS1-STP-FHRP.png), [18 – Office A DS2 STP/FHRP](../ScreenShots/18-Office-A-DS2-STP-FHRP.png), [19 – Office B DS1 STP/FHRP](../ScreenShots/19-Office-B-DS1-STP-FHRP.png), [20 – Office B DS2 STP/FHRP](../ScreenShots/20-Office-B-DS2-STP-FHRP.png).
