# Service: pfSense

Role:
- VLAN termination and inter-VLAN routing
- Firewall policy enforcement
- DHCP/DNS
- NAT to WAN

Key interfaces (concept):
- WAN on vmbr0
- LAN trunk on vmbr1 (VLAN10/20/30/40/50)
- UniFi Mgmt path on vmbr2 (VLAN60)

Operational notes:
- Most “no internet” incidents trace back to NAT coverage or DNS rules.
- Keep aliases for networks/ports to make policy readable and reusable.

