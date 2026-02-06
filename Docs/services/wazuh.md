# Service: Wazuh

Role:
- SIEM/telemetry for endpoints and infrastructure
- Alerting + hunting

Placement:
- Prefer VLAN40 (Monitoring)

Data sources (target):
- Windows endpoints (VLAN50) via agent + Sysmon
- Linux endpoints via agent
- pfSense logs via syslog (optional but valuable)
- UniFi events (optional)

Validation approach:
- Create a small set of repeatable “test detections” and document them in runbooks.
