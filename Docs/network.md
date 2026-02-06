# Network Design

This document is the source of truth for addressing, VLANs, gateways, DHCP scopes, and “what should talk to what”.

## VLANs and IP plan
| VLAN | Name             | Subnet            | Gateway (pfSense)      | DHCP Scope (example)        |
|-----:|------------------|-------------------|------------------------|-----------------------------|
| 10   | LAN/Base         | 192.168.10.0/24   | 192.168.10.1           | 192.168.10.50-199           |
| 20   | Guest            | 192.168.20.0/24   | 192.168.20.1           | 192.168.20.50-199           |
| 30   | IoT              | 192.168.30.0/24   | 192.168.30.1           | 192.168.30.540-199          |
| 40   | Monitoring       | 10.40.0.0/24      | 10.40.0.1              | static/reserved preferred   |
| 50   | Windows/Test     | 10.50.0.0/24      | 10.50.0.1              | 10.50.0.50-199              |
| 60   | UniFi-Management | 192.168.60.0/24   | 192.168.60.1           | static/reserved preferred   |

## Proxmox bridges (intent)
- vmbr0 = WAN (untagged)
- vmbr1 = LAN trunk (tagged VLANs: 10/20/30/40/50; optionally untagged 10 depending on design)
- vmbr2 = management path for VLAN60 (either tagged-only or dedicated access)

## UniFi SSID → VLAN mapping (example)
- SSID: Guest → VLAN 20
- SSID: IoT → VLAN 30
- SSID: (optional) Internal → VLAN 10

## Firewall policy summary (intent)
- VLAN20 Guest:
  - allow DNS to pfSense
  - allow outbound web (80/443) as needed
  - block access to RFC1918/internal networks
- VLAN30 IoT:
  - allow DNS/NTP
  - allow only required outbound (vendor clouds)
  - deny access to management/monitoring VLANs by default
- VLAN60 UniFi Management:
  - restrict to admin workstation(s) + controller + APs
- VLAN40 Monitoring:
  - allow inbound agents from endpoints
  - restrict outbound to update repos as required

## “Known gotchas” to document as rules
- If VLAN clients get IPs but no internet:
  - check outbound NAT covers that interface
  - check DNS policy (clients must reach resolver)
  - check firewall rule ordering and aliases
