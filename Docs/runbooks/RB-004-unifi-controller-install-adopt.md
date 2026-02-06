# RB-004: UniFi Controller install + AP adoption

Purpose: repeatable UniFi Controller deployment and U6+ adoption.

## Steps (outline)
1) Deploy controller VM/container on VLAN60 (preferred)
2) Ensure controller has stable IP (DHCP reservation)
3) Ensure AP can reach controller (L3 adopt path is fine if routed + allowed)
4) Create networks:
- Guest (VLAN20), IoT (VLAN30), Mgmt (VLAN60)
5) Create SSIDs mapped to VLANs
6) Adopt AP, apply firmware updates

## Validation
- AP shows “Connected”
- client on Guest gets VLAN20 IP and has internet (if allowed)
- client on IoT gets VLAN30 IP and has internet (if allowed)
- management UI not reachable from Guest/IoT
