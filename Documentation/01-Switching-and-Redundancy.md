# Switching and Redundancy Configuration Reference

This guide records the switching and first-hop redundancy topics visible in the Packet Tracer evidence. Device names and interface IDs below follow the screenshots where they are readable. Apply the examples only to the corresponding links/devices in the topology.

## VLAN segmentation

The screenshots show these VLANs:

| VLAN | Name / purpose | Office |
|---:|---|---|
| 10 | PCs | A and B |
| 20 | Phones | A and B |
| 30 | Office B user/data segment | B |
| 40 | Wi-Fi | A |
| 99 | Management | A and B |

Office A screenshots show VLANs 10, 20, 40 and 99. Office B security evidence shows VLANs 10, 20, 30 and 99.

Example VLAN creation:

```cisco
configure terminal
vlan 10
 name pcs
vlan 20
 name phones
vlan 40
 name wifi
vlan 99
 name management
end
show vlan brief
```

For Office B, create VLAN 30 as well and use the VLAN list shown for that office.

## Access ports and 802.1Q trunking

Access ports place an endpoint in one VLAN. Trunk links carry multiple VLANs between network devices. The Office A trunk evidence shows native VLAN 1000 and allowed VLANs 10, 20, 40 and 99.

```cisco
interface <access-interface>
 switchport mode access
 switchport access vlan <vlan-id>

interface <trunk-interface>
 switchport mode trunk
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 10,20,40,99
```

Useful checks:

```cisco
show interfaces trunk
show interfaces <interface-id> switchport
```

Screenshot references: [08 – Office A Trunking](../ScreenShots/08-Office-A-Trunking.png), [09 – Office B Trunking](../ScreenShots/09-Office-B-Trunking.png).

## DTP and VTP

The DTP evidence confirms trunk negotiation is disabled on the displayed trunk interface (Negotiation of Trunking: Off). A static trunk is used in that case.

VTP status evidence shows VTP Version 2, domain `JeremySITLab`, and the displayed switch operating as a server.

Reference commands:

```cisco
show interfaces <interface-id> switchport
show vtp status
show vtp counters
```

Use the same VTP domain and version on participating switches. Configure the intended VTP role deliberately; do not change a production VTP domain or revision without planning.

Screenshot references: [10 – Office A DTP](../ScreenShots/10-Office-A-DTP.png), [11 – Office B DTP](../ScreenShots/11-Office-B-DTP.png), [12 – Office A VTP](../ScreenShots/12-Office-A-VTP.png), [13 – Office B VTP](../ScreenShots/13-Office-B-VTP.png).

## EtherChannel

The lab evidence shows three EtherChannel implementations:

- Office A: PAgP, Port-channel 1, member interfaces GigabitEthernet1/0/4 and GigabitEthernet1/0/5.
- Office B: LACP, Port-channel 1, member interfaces GigabitEthernet1/0/4 and GigabitEthernet1/0/5.
- Core Layer 3 bundle: Port-channel 1, using PAgP in the captured output, with member interfaces GigabitEthernet1/0/2 and GigabitEthernet1/0/3. The displayed Port-channel IP is 10.0.0.42.

Representative member configuration patterns:

```cisco
! PAgP example – use matching settings on both ends
interface range gigabitEthernet1/0/4-5
 channel-group 1 mode desirable

! LACP example – use matching settings on both ends
interface range gigabitEthernet1/0/4-5
 channel-group 1 mode active
```

For a Layer 3 EtherChannel, configure the physical members and Port-channel as routed interfaces according to the topology's addressing plan:

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

A healthy bundled Layer 2 port-channel is shown as `SU` in the captured output; the Layer 3 bundle is shown as `RU`. Member ports are marked `(P)`.

Screenshot references: [14 – Office A PAgP](../ScreenShots/14-Office-A-PAgP-EtherChannel.png), [15 – Office B LACP](../ScreenShots/15-Office-B-LACP-EtherChannel.png), [16 – Layer 3 EtherChannel](../ScreenShots/16-Layer-3-EtherChannel.png).

## Rapid PVST+ and HSRPv2

The screenshots show Rapid PVST+ spanning-tree operation and HSRP version 2 gateway redundancy. Office A uses VLAN-specific root priorities and HSRP virtual gateways. The captured Office A HSRP output includes virtual IPs 10.1.0.1, 10.2.0.1, 10.6.0.1 and 10.0.0.1 for VLANs 10, 20, 40 and 99 respectively.

A representative HSRP configuration pattern from the displayed configuration:

```cisco
interface vlan 10
 standby version 2
 standby 2 ip 10.1.0.1
 standby 2 priority 105
 standby 2 preempt
```

Use the matching HSRP group and virtual IP for each VLAN, with active/standby priorities coordinated between the two distribution switches. The screenshot shows the device configured with `standby 1 ip 10.0.0.1`, priority 105 and preemption on one SVI; do not assume one HSRP group number is used for every VLAN.

Verification:

```cisco
show standby brief
show spanning-tree vlan <vlan-id>
show spanning-tree
```

Screenshot references: [17 – Office A DS1 STP/FHRP](../ScreenShots/17-Office-A-DS1-STP-FHRP.png), [18 – Office A DS2 STP/FHRP](../ScreenShots/18-Office-A-DS2-STP-FHRP.png), [19 – Office B DS1 STP/FHRP](../ScreenShots/19-Office-B-DS1-STP-FHRP.png), [20 – Office B DS2 STP/FHRP](../ScreenShots/20-Office-B-DS2-STP-FHRP.png).
