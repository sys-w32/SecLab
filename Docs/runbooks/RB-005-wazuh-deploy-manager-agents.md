# RB-005: Wazuh deploy (manager/indexer/dashboard) + agent onboarding

Purpose: standard Wazuh deployment and agent enrollment for Windows/Linux.
* Make sure the server IP is the IP of the Wazuh Manager

## Steps (outline)
1) Deploy Wazuh on VLAN40 (preferred)
2) Confirm dashboard access restricted to trusted VLAN/admin host
3) Configure ingestion sources:
- Wazuh agents (Windows/Linux)
- pfSense syslog → Wazuh (if configured)
- UniFi events (if configured)

## Agent onboarding checklist
- Install agent
- Enroll with manager
- Confirm agent shows “Active”
- Generate test events:
  - Linux: auth failures / sudo events
  - Windows: login failures + Sysmon (recommended)

## Validation
- Wazuh shows logs flowing from each endpoint
- Alerts triggered for a known test rule
