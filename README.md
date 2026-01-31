# Cybersecurity Home Lab (Proxmox VE + pfSense + Unifi + Wazuh) 

This repo documents a segmented home security lab for both blue-team and red team skill building: network segmentation, firewall policy, 
endpoint telemetry, and troubleshooting. 
The environment is centered on Proxmox VE hosting pfSense (routing/firewall/VLAN termination), UniFi Wi-Fi (U6+ AP + Controller), and Wazuh (SIEM/endpoint telemetry).

Primary Goals:
 - Build a realistic multi-VLAN environment (Guest, IoT, Management, Windows, VPN, and Monitoring)
 - Practice firewalling, NAT, DNS hygiene, and service isolation
 - Centralize visibility ( pfSense + Unifi + endpoints) into Wazuh
 - Produce documentation and runbooks for troubleshooting and assisting

## Quick Links
- Architecture: `docs/architecture.md`
- Network design: `docs/network.md`
- Security posture: `docs/security.md`
- Runbooks: `docs/runbooks/`
- Services: `docs/services/`
- Troubleshooting: `docs/troubleshooting/`

## Lab Stack 
- Hypervisor: Proxmox VE
- Firewall/Router: pfSense (VVLAN termination, DHCP/DNS, NAT)
- Wi-Fi: UniFi U6+ AP (PoE) + Unifi Controller
- Monitoring/SIEM: Wazuh (Manager/Indexer/Dashboard + Agents)

  ## VLANs (current)
| VLAN | Name            | Subnet (example)        | Purpose |
|-----:|-----------------|--------------------------|---------|
| 10   | LAN/Base        | 192.168.10.0/24          | Trusted/internal |
| 20   | Guest           | 192.168.20.0/24          | Guest Wi-Fi, heavily restricted |
| 30   | IoT             | 192.168.30.0/24          | IoT devices, restricted egress |
| 40   | Monitoring      | 10.40.0.0/24             | Wazuh/SOC tooling |
| 50   | Windows/Test    | 10.50.0.0/24             | Windows endpoints/test range |
| 60   | UniFi-Management| 192.168.60.0/24          | UniFi management plane |

## Proxmox bridges (current model)
- vmbr0: WAN (pfSense WAN)
- vmbr1: LAN trunk for VLANs 10/20/30/40/50 (tagged)
- vmbr2: dedicated management bridge for VLAN 60 (UniFi mgmt isolation)

## Conventions
- Use aliases in pfSense for network objects and port groups.
- Document every change as either:
  - a runbook update (repeatable procedure), or
  - a troubleshooting entry (symptoms → root cause → fix → prevention)

## Changelog
See `CHANGELOG.md`.
