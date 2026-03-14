# Architecture

This lab is designed as a segmented environment where pfSense is the control plane for routing, security policy, DHCP/DNS, and NAT. UniFi provides Wi-Fi SSIDs mapped to VLANs. Wazuh aggregates endpoint and network telemetry.

## High-level components
- Proxmox VE: hosts VMs and Linux bridges (vmbr0/vmbr1/vmbr2)
- pfSense VM: WAN edge, VLAN termination, firewall rules, NAT, DHCP/DNS
- UniFi Controller: manages AP + SSIDs/VLAN mappings
- UniFi U6+ AP: broadcasts SSIDs for Guest (VLAN20) and IoT (VLAN30) (and optionally LAN)
- Wazuh: endpoint agents + indexing/dashboard, central visibility

## Data flows (intended)
- Guest/IoT clients → pfSense → WAN (restricted egress + DNS policy)
- Endpoints → Wazuh (agent telemetry, auth logs, Sysmon where applicable)
- pfSense logs → Wazuh (firewall/DNS/NAT insight)
- UniFi events → Wazuh (client associations, admin actions, device events)

## Port Mapping
- Host NIC: Intel i350-T4 Quad-Port
- Port Map
  - Port 0: n/a
  - Port 1: Laptop LAN Connection ( Will change later )
  - Port 2: Unifi U6+ PoE AP (vmbr2)
  - Port 3: Homeplug > Router 

## Diagram (Mermaid)
```mermaid
flowchart LR
  ISP((ISP Router/Modem)) -->|WAN| PVE[Proxmox VE]
  PVE -->|"vmbr0 (WAN)"| PFS[pfSense VM]

  PVE -->|"vmbr1 (LAN trunk)"| PFS
  PVE -->|"vmbr2 (VLAN60 mgmt)"| PFS

  PFS -->|"VLAN10/20/30/40/50 gateways"| NETS[(VLAN Networks)]
  NETS --> UCTRL[UniFi Controller]
  UCTRL --> UAP[UniFi U6+ AP]

  NETS --> WAZ[Wazuh Manager/Indexer/Dashboard]
  NETS --> WIN[Windows VMs/Hosts]
  NETS --> LNX[Linux VMs/Hosts]
