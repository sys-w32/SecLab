# RB-006: Windows 11 VM provisioning (VLAN50)

Purpose: consistent Windows test endpoint creation for telemetry + attack/detect exercises.

## Steps (outline)
1) Create VM in Proxmox (UEFI + TPM if required)
2) Place vNIC on VLAN50
3) Install Windows 11
4) Baseline hardening:
- local admin hygiene
- updates
- disable unnecessary services
5) Telemetry:
- install Wazuh agent
- install Sysmon (recommended) + validate logs

## Validation
- VM gets VLAN50 IP
- Wazuh sees endpoint + Sysmon events (if enabled)
