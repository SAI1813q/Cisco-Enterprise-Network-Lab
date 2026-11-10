# Project Configuration Documentation

These reference guides describe the configuration areas implemented in the Cisco Enterprise Network Lab. They are based on the CLI output and Packet Tracer screenshots stored in the `ScreenShots/` directory.

## Guides

1. [Switching and Redundancy](01-Switching-and-Redundancy.md) — VLANs, access/trunk ports, DTP, VTP, PAgP/LACP EtherChannel, Rapid PVST+ and HSRPv2.
2. [Routing and IPv6](02-Routing-and-IPv6.md) — Layer 3 interfaces, OSPF Area 0, static/floating routes and IPv6.
3. [Security and Edge Controls](03-Security-and-Edge-Controls.md) — DHCP snooping, Dynamic ARP Inspection, port security, ACLs and NAT/PAT.
4. [Network Services and Management](04-Network-Services-and-Management.md) — DHCP, DNS, Syslog, NTP, SSH and FTP.
5. [Wireless and Neighbor Discovery](05-Wireless-and-Neighbor-Discovery.md) — WLC, WLAN/client association and LLDP.

## How to use these references

- The guides are documentation and learning references, not universal copy/paste templates.
- Replace placeholders such as `<interface-id>`, `<vlan-id>` and `<REDACTED>` with values appropriate to the device and topology.
- Interface assignments, ACL direction, routing statements and trusted-port placement must match the actual network design.
- The screenshot links point to the corresponding CLI or service evidence in the repository.
- Credentials, pre-shared keys and other secrets are intentionally omitted from the written guides. Redact them from public screenshots before publishing.

## Lab files

See the repository's [Packet Tracer files](../Packet-Tracer/) and [screenshot evidence](../ScreenShots/).
