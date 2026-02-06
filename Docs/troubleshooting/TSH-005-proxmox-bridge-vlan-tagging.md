# TSH-005: Proxmox bridge VLAN tagging problems

Symptoms:
- VLAN interface in pfSense is up, but no clients get leases
- UniFi SSID connects but clients land in wrong subnet
- Only “any to any” rules appear to make things work (masking a tagging issue)

Checks:
- vmbr1/vmbr2 are VLAN-aware
- trunk interface carries the VLAN IDs you expect
- pfSense vNIC is attached to the correct bridge
- UniFi SSID VLAN tag matches the pfSense VLAN

Fix:
- Enable VLAN-aware on the bridge
- Correct allowed VLAN list
- Correct which physical NIC is attached to which bridge
