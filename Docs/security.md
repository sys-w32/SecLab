# Security

Security posture for the lab: baseline controls, hardening, logging, and operational practices.

## Threat model (practical)
Primary risks:
- Guest/IoT lateral movement into trusted VLANs
- Exposure of management planes (pfSense, UniFi, hypervisor)
- Weak credentials / password reuse
- Blind spots (missing logs/telemetry)

## Core controls
Network:
- VLAN segmentation with default-deny between VLANs
- Explicit allow rules only (documented by purpose)
- Management plane isolated (VLAN60)
- Monitoring stack isolated (VLAN40)

Identity/Access:
- Unique admin creds per platform
- MFA where available (UniFi, any cloud management)
- Least-privilege admin accounts (separate daily vs admin)

Endpoint:
- Wazuh agents on Windows/Linux where possible
- Sysmon on Windows test VLAN (recommended)
- Time sync (NTP) consistent across VLANs

Logging/Visibility:
- pfSense logs exported/ingested
- UniFi events ingested where feasible
- Wazuh rules tuned and mapped to ATT&CK where relevant

## Secrets handling (repo hygiene)
- Do NOT commit:
  - pfSense config exports with secrets
  - API keys, private keys, tokens, passwords
- Recommended approach:
  - store sanitized configs in `configs/`
  - keep real secrets in a password manager or encrypted vault
  - if you must track config changes, redact sensitive fields first

## Baseline hardening checklist
- pfSense:
  - restrict GUI to mgmt VLAN only
  - disable WAN admin access
  - backup configs securely
- Proxmox:
  - restrict web UI to trusted VLAN/admin host
  - keep updated, limit SSH exposure
- UniFi:
  - controller on mgmt VLAN
  - restrict who can reach controller UI
- Wazuh:
  - expose dashboard only to trusted networks
  - ship logs over secure channels where possible

