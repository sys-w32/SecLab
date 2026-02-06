# Service: UniFi Controller

Role:
- Manages UniFi AP + SSIDs + VLAN mappings
- Provides visibility for Wi-Fi clients and device health

Placement:
- Prefer VLAN60 (management plane)

Notes:
- If clients join Wi-Fi but have no internet, verify:
  - SSID VLAN tagging
  - pfSense VLAN interface + DHCP
  - NAT + DNS allow rules
