# Wireless and Neighbor Discovery Reference

## Wireless LAN Controller

The screenshots show a Cisco 3504 Wireless LAN Controller managed through its web interface at 10.0.0.7. The controller summary identifies the device as WLC1.

The interface list shows:

| Interface | VLAN | IP address | Type |
|---|---:|---|---|
| Wi-Fi | 40 | 10.6.0.4 | Dynamic |
| management | untagged | 10.0.0.7 | Static |
| virtual | N/A | 192.0.2.1 | Static |

The WLAN list shows WLAN ID 1, profile name `Wi-Fi`, SSID `Wi-Fi`, enabled status and WPA2-PSK security.

The wireless client screenshot shows a laptop associated with the Wi-Fi SSID and receiving an IPv4 address through DHCP. The client uses the Wi-Fi VLAN/subnet configured in the lab.

## Configuration workflow

1. Configure the WLC management interface and management reachability.
2. Create the dynamic Wi-Fi interface and associate it with VLAN 40.
3. Create WLAN ID 1 and set the profile/SSID to `Wi-Fi`.
4. Enable the WLAN and configure WPA2-PSK.
5. Associate the wireless client using the SSID and the lab's locally configured key.
6. Confirm the client receives the expected address, subnet mask and default gateway.

The WLAN pre-shared key is deliberately not reproduced in this document. Keep it lab-only and redact it from public screenshots.

## Evidence

- [58 – Wireless LAN Controller](../ScreenShots/58-Wireless-LAN-Controller.png)
- [59 – WLC Interface Configuration](../ScreenShots/59-WLC-Interface-Configuration.png)
- [60 – WLC WLAN Configuration](../ScreenShots/60-WLC-WLAN-Configuration.png)
- [61 – Wireless Client Connection](../ScreenShots/61-Wireless-Client-Connection.png)

## LLDP neighbor discovery

The LLDP screenshot shows the command `show lldp neighbors` executed on an Office B access switch. Two router-capable neighbors are displayed, with local and remote interface identifiers.

Verification:

```cisco
show lldp neighbors
show lldp neighbors detail
```

Screenshot reference: [63 – LLDP Neighbor Discovery](../ScreenShots/63-LLDP-Neighbor-Discovery.png).
