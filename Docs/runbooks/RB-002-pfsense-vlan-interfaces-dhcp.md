# RB-002: pfSense VLAN interfaces + DHCP

Purpose: add/verify VLAN interfaces and DHCP scopes.

## Procedure
1) Create VLANs on the LAN parent interface
- VLAN10/20/30/40/50 on trunk (vmbr1)
- VLAN60 on mgmt path (vmbr2) if dedicated

2) Assign interfaces
- VLAN20_GUEST, VLAN30_IOT, VLAN40_WAZUH, VLAN50_WINDOWS, VLAN60_UNIFI_MGMT (+ LAN/Base)

3) Configure interface IPs /24 (per `docs/network.md`)

4) DHCP scopes
- enable DHCP for client VLANs (10/20/30/50)
- prefer static/reservations for infrastructure VLANs (40/60)

## Validation
- client receives correct lease + gateway + DNS
- client can ping gateway
- DNS resolution works from client
