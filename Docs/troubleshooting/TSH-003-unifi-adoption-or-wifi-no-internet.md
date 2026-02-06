# TSH-003: UniFi adoption issues / Wi-Fi connects but no internet

Symptoms:
- AP shows offline/adoption loop OR
- Client connects to SSID but no internet

Checks:
- Controller and AP can reach each other (L3 permitted)
- SSID VLAN ID matches the VLAN in pfSense and trunk/tagging
- DHCP scope exists for that VLAN and is enabled
- NAT and DNS rules exist for that VLAN
- Management UI is not accidentally exposed to Guest/IoT

Fix:
- Correct SSID VLAN tag
- Confirm VLAN interface + DHCP + NAT on pfSense
- Confirm trunk carries VLANs where needed
