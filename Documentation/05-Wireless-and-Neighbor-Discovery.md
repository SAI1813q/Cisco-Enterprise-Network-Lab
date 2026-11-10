# 📶 Wireless and Neighbor Discovery Reference

## 📡 Wireless LAN Controller

The screenshots show a Cisco 3504 Wireless LAN Controller managed through its web interface at 10.0.0.7. The controller summary identifies the device as WLC1.

The WLC interface screenshot shows a dynamic interface named Wi-Fi associated with VLAN 40 and IP address 10.6.0.4. The management interface is statically configured as 10.0.0.7. The virtual interface is shown as 192.0.2.1.

The WLAN list shows WLAN ID 1, profile name `Wi-Fi`, SSID `Wi-Fi`, enabled status and WPA2-PSK security.

## ⚙️ Configuration workflow

1. Configure the WLC management interface and management reachability.
2. Create the dynamic Wi-Fi interface and associate it with VLAN 40.
3. Create WLAN ID 1 and set the profile and SSID to `Wi-Fi`.
4. Enable the WLAN and configure WPA2-PSK.
5. Configure the wireless client to use the `Wi-Fi` SSID and the lab's locally configured key.

The wireless client screenshot records the client wireless settings and SSID. **The IPv4 address displayed in that screenshot is not evidence of a successful Wi-Fi VLAN DHCP lease.** Packet Tracer displayed an incorrect address because of a simulator error in this lab. Therefore, this screenshot is retained as evidence of the client-side wireless configuration only; it is not used to claim successful DHCP assignment or end-to-end wireless connectivity.

The WLAN pre-shared key is not reproduced in this written guide. The public screenshots are retained unchanged as requested, so treat any visible wireless key as exposed and lab-only. Do not reuse it elsewhere.

## Evidence

- [58 – Wireless LAN Controller](../ScreenShots/58-Wireless-LAN-Controller.png)
- [59 – WLC Interface Configuration](../ScreenShots/59-WLC-Interface-Configuration.png)
- [60 – WLC WLAN Configuration](../ScreenShots/60-WLC-WLAN-Configuration.png)
- [61 – Wireless Client Connection](../ScreenShots/61-Wireless-Client-Connection.png) — client wireless settings; displayed IPv4 address affected by a Packet Tracer simulator error.

## 🔎 LLDP neighbor discovery

The LLDP screenshot shows `show lldp neighbors` executed on an Office B access switch. Two router-capable neighbors are displayed with local and remote interface identifiers.

```cisco
show lldp neighbors
show lldp neighbors detail
```

Screenshot reference: [63 – LLDP Neighbor Discovery](../ScreenShots/63-LLDP-Neighbor-Discovery.png).
