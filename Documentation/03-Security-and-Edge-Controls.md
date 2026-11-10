# Security and Edge Controls Configuration Reference

This guide documents the Layer 2 protections, access-control lists and NAT evidence included in the repository.

## DHCP snooping

The Office A screenshot shows DHCP snooping enabled for VLANs 10, 20, 40 and 99. Office B uses VLANs 10, 20, 30 and 99.

```cisco
ip dhcp snooping
ip dhcp snooping vlan 10,20,40,99
no ip dhcp snooping information option
```

For Office B, use the Office B VLAN list shown in its screenshot. Mark only the legitimate DHCP-server-facing/uplink interfaces as trusted; keep client-facing access ports untrusted. Trust settings must follow the actual cabling and DHCP-server location.

Verification:

```cisco
show ip dhcp snooping
show ip dhcp snooping binding
```

Screenshot references: [33 – DHCP Snooping](../ScreenShots/33-DHCP-Snooping.png), [35 – Office A ARP Inspection and DHCP Snooping Config](../ScreenShots/35-Office-A-ARP-Inspection-and-DHCP-Snooping-Config.png), [37 – Office B ARP Inspection and DHCP Snooping Config](../ScreenShots/37-Office-B-ARP-Inspection-and-DHCP-Snooping-Config.png).

## Dynamic ARP Inspection (DAI)

The screenshots show DAI enabled on Office A VLANs 10, 20, 40 and 99 and Office B VLANs 10, 20, 30 and 99. The validation options shown are source MAC, destination MAC and IP address.

```cisco
ip arp inspection vlan 10,20,40,99
ip arp inspection validate src-mac dst-mac ip
```

Use the Office B VLAN list on Office B switches. DAI relies on DHCP snooping bindings for dynamically addressed clients; configure trusted interfaces consistently with the DHCP-snooping design.

Verification:

```cisco
show ip arp inspection vlan 10,20,40,99
show ip arp inspection statistics
```

Screenshot references: [34 – Office A Dynamic ARP Inspection](../ScreenShots/34-Office-A-Dynamic-ARP-Inspection.png), [35 – Office A DAI/DHCP Snooping Config](../ScreenShots/35-Office-A-ARP-Inspection-and-DHCP-Snooping-Config.png), [36 – Office B Dynamic ARP Inspection](../ScreenShots/36-Office-B-Dynamic-ARP-Inspection.png), [37 – Office B DAI/DHCP Snooping Config](../ScreenShots/37-Office-B-ARP-Inspection-and-DHCP-Snooping-Config.png).

## Switch port security

The Office A and Office B evidence shows port security enabled on FastEthernet0/1, with one allowed secure MAC address, sticky learning and violation mode `restrict`.

```cisco
interface fastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
```

Verification:

```cisco
show port-security
show port-security interface fastEthernet0/1
show port-security address
```

Screenshot references: [38 – Office A Port Security](../ScreenShots/38-Office-A-Port-Security.png), [39 – Office B Port Security](../ScreenShots/39-Office-B-Port-Security.png).

## Standard and extended ACLs

The screenshot shows an extended named ACL called `OfficeA_to_OfficeB` with the following entries:

```cisco
ip access-list extended OfficeA_to_OfficeB
 10 permit icmp 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 20 deny ip 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 30 permit ip any any
```

This permits ICMP from the 10.1.0.0/24 network to 10.3.0.0/24, denies other IP traffic between those two subnets, and permits other traffic matched by the final entry. ACL direction and interface placement determine where the policy takes effect; refer to the device configuration before changing it.

The screenshots also show standard ACL entries:

```cisco
access-list 1 permit 10.1.0.0 0.0.0.255
access-list 2 permit 10.2.0.0 0.0.0.255
access-list 2 permit 10.1.0.0 0.0.0.255
access-list 2 permit 10.3.0.0 0.0.0.255
access-list 2 permit 10.4.0.0 0.0.0.255
access-list 2 permit 10.6.0.0 0.0.0.255
```

Verification:

```cisco
show access-lists
show ip interface
```

Screenshot references: [40 – Standard and Extended ACLs](../ScreenShots/40-Standard-and-Extended-ACLs.png), [41 – Office A ACLs](../ScreenShots/41-Office-A-ACLs.png), [42 – R1 ACLs](../ScreenShots/42-R1-ACLs.png).

## NAT and PAT

The NAT evidence shows a pool named `POOL1` with addresses 203.0.113.200–203.0.113.207 and a /29 mask, using standard ACL 2 as the inside-source selector. The translation table also shows a static mapping from inside-local 10.5.0.4 to inside-global 203.0.113.113.

Representative configuration pattern:

```cisco
ip nat pool POOL1 203.0.113.200 203.0.113.207 netmask 255.255.255.248
ip nat inside source list 2 pool POOL1
ip nat inside source static 10.5.0.4 203.0.113.113
```

The `ip nat inside` and `ip nat outside` roles must be assigned to the correct interfaces based on the topology.

Verification:

```cisco
show ip nat statistics
show ip nat translations
```

Screenshot reference: [43 – NAT and PAT](../ScreenShots/43-NAT-and-PAT.png).

## Security configuration hygiene

The uploaded screenshots include credential/key fields in the FTP, NTP and wireless configuration evidence. Before keeping the repository public, replace those values in the screenshots with redacted versions and use non-production lab-only credentials. Do not publish reusable passwords, pre-shared keys or authentication secrets.
