# Network Services and Device Management Reference

This document summarizes the services visible in the lab evidence: DHCP, DNS, Syslog, NTP, SSH and FTP.

## DHCP pools

The R1 configuration screenshot shows excluded address ranges and pools for management, PC, phone and Wi-Fi networks. The following entries are readable in the screenshot:

```cisco
ip dhcp excluded-address 10.0.0.1 10.0.0.10
ip dhcp excluded-address 10.1.0.1 10.1.0.10
ip dhcp excluded-address 10.2.0.1 10.2.0.10
ip dhcp excluded-address 10.3.0.1 10.3.0.10
ip dhcp excluded-address 10.4.0.1 10.4.0.10
ip dhcp excluded-address 10.6.0.1 10.6.0.10

ip dhcp pool A-Mgmt
 network 10.0.0.0 255.255.255.240
 default-router 10.0.0.1
 option 43 ip 10.0.0.7
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool A-pc
 network 10.1.0.0 255.255.255.0
 default-router 10.1.0.1
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool A-phone
 network 10.2.0.0 255.255.255.0
 default-router 10.2.0.1
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool B-mgmt
 network 10.0.0.16 255.255.255.240
 default-router 10.0.0.17
 option 43 ip 10.0.0.7
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool B-phone
 network 10.4.0.0 255.255.255.0
 default-router 10.4.0.1
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool B-pc
 network 10.3.0.0 255.255.255.0
 default-router 10.3.0.1
 dns-server 10.5.0.4
 domain-name jeremysitlab.com

ip dhcp pool wi-fi
 network 10.6.0.0 255.255.255.0
 default-router 10.6.0.1
 dns-server 10.5.0.4
 domain-name jeremysitlab.com
```

Verification:

```cisco
show ip dhcp binding
show ip dhcp pool
show ip dhcp conflict
```

Screenshot references: [31 – DHCP Pool Configuration](../ScreenShots/31-DHCP-Pool-Configuration.png), [32 – DHCP Bindings](../ScreenShots/32-DHCP-Bindings.png), [62 – Additional PC1 Configuration](../ScreenShots/62-Additional-PC1-Configuration.png).

## DNS

The server screenshot shows the DNS service enabled on SRV1, with IPv4 address 10.5.0.4/24. The resource-record screenshot shows these entries:

| Name | Record type | Value |
|---|---|---|
| google.com | A | 172.253.62.100 |
| jeremysitlab.com | A | 66.235.200.145 |
| www.jeremysitlab.com | CNAME | jeremysitlab.com |
| youtube.com | A | 152.250.31.93 |

These are the values captured in the Packet Tracer DNS service; they are lab records and should not be treated as authoritative public DNS records.

Client-side check shown in the evidence:

```text
ping youtube.com
```

Screenshot references: [51 – DNS Server](../ScreenShots/51-DNS-Server.png), [52 – DNS Mapping](../ScreenShots/52-DNS-Mapping.png), [53 – PC1 DNS Resolution Test](../ScreenShots/53-PC1-DNS-Resolution-Test.png).

## Syslog

The Syslog service is enabled on SRV1. The server address shown in the screenshots is 10.5.0.4. R1's logging output shows messages being sent to host 10.5.0.4 on UDP port 514.

Representative router command:

```cisco
logging host 10.5.0.4
```

Verification:

```cisco
show logging
```

Screenshot references: [47 – R1 Syslog Configuration](../ScreenShots/47-R1-Syslog-Configuration.png), [48 – Syslog Server](../ScreenShots/48-Syslog-Server.png).

## NTP

The NTP screenshots show authenticated NTP configuration. R1 is configured as an NTP master at stratum 5 and also has an upstream NTP server entry. The client screenshot shows a client using R1 at 10.0.0.76.

Example with the secret deliberately redacted:

```cisco
! Time source / master
ntp authentication-key 1 md5 <REDACTED>
ntp authenticate
ntp trusted-key 1
ntp server 216.239.35.0
ntp master 5

! Client device
ntp authentication-key 1 md5 <REDACTED>
ntp trusted-key 1
ntp server 10.0.0.76 key 1
```

Use the same key ID and matching secret on the participating devices. Never commit the actual authentication key to a public repository.

Verification:

```cisco
show ntp status
show ntp associations
```

Screenshot references: [49 – NTP Master and Client](../ScreenShots/49-NTP-Master-and-Client.png), [50 – NTP Client](../ScreenShots/50-NTP-Client.png).

## SSH management access

The SSH screenshot shows SSH version 2 and VTY lines configured for local login, synchronous logging and SSH-only inbound transport. The VTY ranges are 0–4 and 5–15.

```cisco
ip ssh version 2
line vty 0 4
 login local
 logging synchronous
 transport input ssh
line vty 5 15
 login local
 logging synchronous
 transport input ssh
```

The local user, domain name, RSA key generation and management ACL must also be configured as required by the device setup. Do not copy passwords or private keys into this repository.

Verification:

```cisco
show ip ssh
show running-config | section line vty
```

Screenshot references: [44 – SSH Configuration](../ScreenShots/44-SSH-Configuration.png), [45 – PC1 SSH Access](../ScreenShots/45-PC1-SSH-Access.png), [46 – PC2 SSH Access](../ScreenShots/46-PC2-SSH-Access.png).

## FTP file transfer and boot workflow

The FTP screenshots show the FTP service enabled on SRV1, a file listing, and the router's system image information before and after the demonstrated workflow. The router-side configuration uses FTP credentials, but the actual credential values are intentionally omitted here.

Generic command pattern (replace placeholders locally; do not commit credentials):

```cisco
ip ftp username <FTP_USERNAME>
ip ftp password <FTP_PASSWORD>
```

The before/after screenshots show the IOS image filename and version information changing after the FTP-based file workflow. They are evidence of the demonstrated lab process; this reference intentionally does not prescribe a boot command that is not legible in the captured output.

Screenshot references: [54 – FTP User Credentials](../ScreenShots/54-FTP-User-Credentials.png), [55 – FTP File Transfer](../ScreenShots/55-FTP-File-Transfer.png), [56 – FTP Boot Before System Info](../ScreenShots/56-FTP-Boot-Before-System-Info.png), [57 – FTP Boot After System Info](../ScreenShots/57-FTP-Boot-After-System-Info.png).
