# Endpoints

Windows (VLAN50):
- used for detection exercises and tooling
- should run Wazuh agent + Sysmon (recommended)

Linux (various VLANs):
- used for services, testing, and infra roles
- should run Wazuh agent where it makes sense

Conventions:
- stable hostnames
- DHCP reservations for infrastructure nodes
