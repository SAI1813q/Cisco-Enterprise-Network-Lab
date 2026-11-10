# 🛡️ Security and Edge Controls Configuration Reference

This guide documents the Layer 2 protections, access-control lists and NAT evidence included in the repository.

## 🔐 DHCP snooping

The Office A screenshot shows DHCP snooping enabled for VLANs 10, 20, 40 and 99. Office B uses VLANs 10, 20, 30 and 99. In Office B, VLAN 30 is the server VLAN for SRV1; it is not the Wi-Fi VLAN.

Office A command pattern:

```cisco
ip dhcp snooping
ip dhcp snooping vlan 10,20,40,99
no ip dhcp snooping information option
```

Office B command pattern:

```cisco
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,99
no ip dhcp snooping information option
```

Mark only legitimate DHCP-server-facing/uplink interfaces as trusted; keep client-facing access ports untrusted. Trust settings must follow the actual cabling and DHCP-server location.

Verification:

```cisco
show ip dhcp snooping
show ip dhcp snooping binding
```

Screenshot references: [33 – DHCP Snooping](../ScreenShots/33-DHCP-Snooping.png), [35 – Office A ARP Inspection and DHCP Snooping Config](../ScreenShots/35-Office-A-ARP-Inspection-and-DHCP-Snooping-Config.png), [37 – Office B ARP Inspection and DHCP Snooping Config](../ScreenShots/37-Office-B-ARP-Inspection-and-DHCP-Snooping-Config.png).

## 🕵️ Dynamic ARP Inspection (DAI)

The screenshots show DAI enabled on Office A VLANs 10, 20, 40 and 99 and Office B VLANs 10, 20, 30 and 99. The validation options shown are source MAC, destination MAC and IP address.

Office A command pattern:

```cisco
ip arp inspection vlan 10,20,40,99
ip arp inspection validate src-mac dst-mac ip
```

Office B command pattern:

```cisco
ip arp inspection vlan 10,20,30,99
ip arp inspection validate src-mac dst-mac ip
```

DAI relies on DHCP snooping bindings for dynamically addressed clients; configure trusted interfaces consistently with the DHCP-snooping design.

Verification:

```cisco
show ip arp inspection vlan <vlan-id>
show ip arp inspection statistics
```

Screenshot references: [34 – Office A Dynamic ARP Inspection](../ScreenShots/34-Office-A-Dynamic-ARP-Inspection.png), [35 – Office A DAI/DHCP Snooping Config](../ScreenShots/35-Office-A-ARP-Inspection-and-DHCP-Snooping-Config.png), [36 – Office B Dynamic ARP Inspection](../ScreenShots/36-Office-B-Dynamic-ARP-Inspection.png), [37 – Office B DAI/DHCP Snooping Config](../ScreenShots/37-Office-B-ARP-Inspection-and-DHCP-Snooping-Config.png).

## 🔒 Switch port security

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

## 🚦 Standard and extended ACLs

The screenshot shows an extended named ACL called `OfficeA_to_OfficeB`:

```cisco
ip access-list extended OfficeA_to_OfficeB
 10 permit icmp 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 20 deny ip 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
 30 permit ip any any
```

This permits ICMP from 10.1.0.0/24 to 10.3.0.0/24, denies other IP traffic between those subnets, and permits other traffic matched by the final entry. ACL direction and interface placement determine where the policy takes effect.

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

## 🌐 NAT and PAT

The NAT evidence shows a pool named `POOL1` configured with 203.0.113.200–203.0.113.207 and mask 255.255.255.248 (`/29`). Standard ACL 2 selects inside source addresses, and the configuration uses PAT overload. The translation table also shows a static mapping from inside-local 10.5.0.4 to inside-global 203.0.113.113.

**R1 NAT interface roles (running configuration):**

| Interface | NAT role |
|---|---|
| `GigabitEthernet0/0` | `ip nat inside` |
| `GigabitEthernet0/1` | `ip nat inside` |
| `GigabitEthernet0/0/0` | `ip nat outside` |
| `GigabitEthernet0/1/0` | `ip nat outside` |

The supplied R1 running-configuration screenshot confirms the `POOL1` definition, ACL 2 pool overload command, and static mapping `10.5.0.4` → `203.0.113.113`. The pool is documented as entered in the lab; its `/29` boundary caveat is explained below.

**Addressing note:** A `/29` subnet has eight total addresses. For the 203.0.113.200/29 block, `.200` is the network address and `.207` is the broadcast address; `.201–.206` are the usable host addresses. The Packet Tracer pool command is documented as configured in the lab and has not been silently changed. For a production-style configuration, use only valid host addresses in the pool.

```cisco
ip nat pool POOL1 203.0.113.200 203.0.113.207 netmask 255.255.255.248
ip nat inside source list 2 pool POOL1 overload
ip nat inside source static 10.5.0.4 203.0.113.113
```

The `ip nat inside` and `ip nat outside` roles must be assigned to the correct interfaces based on the topology.

Verification:

```cisco
show ip nat statistics
show ip nat translations
```

Screenshot reference: [43 – NAT and PAT](../ScreenShots/43-NAT-and-PAT.png).

## ⚠️ Public screenshot credential note

The screenshots are retained unchanged as requested and may visibly contain lab FTP credentials, an NTP authentication key and wireless security information. Treat these values as public and lab-only: never reuse them on real systems. If any value was reused elsewhere, change it immediately. The written configuration examples use placeholders where appropriate.
