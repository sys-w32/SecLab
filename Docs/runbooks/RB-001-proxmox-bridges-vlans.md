# RB-001: Proxmox bridges + VLAN plumbing (vmbr0/vmbr1/vmbr2)

Purpose: standardize how WAN/LAN trunk/management are represented on the Proxmox host.

## Preconditions
- Intel i350-T4 ports cabled (document port-to-purpose)
- pfSense VM exists

## Procedure (high-level)
1) Confirm physical NIC names
- `ip -br link`

2) Confirm bridge membership (host)
- ensure:
  - vmbr0 = WAN uplink NIC (untagged)
  - vmbr1 = LAN trunk NIC (VLAN-aware)
  - vmbr2 = mgmt NIC or dedicated path (VLAN-aware if tagged)

3) Ensure VLAN-aware bridges (where needed)
- vmbr1: VLAN aware = ON, allow VLANs 10/20/30/40/50
- vmbr2: VLAN aware = ON, allow VLAN 60

4) Attach pfSense NICs
- pfSense WAN vNIC → vmbr0
- pfSense LAN trunk vNIC → vmbr1
- pfSense MGMT vNIC (if used) → vmbr2

## Validation
- From pfSense:
  - WAN gets upstream IP
  - VLAN interfaces show “up”
- From a test VM on each VLAN:
  - gets DHCP lease
  - can ping its gateway
  - can resolve DNS
  - can reach internet (if allowed)

## Rollback
- revert the last bridge change
- restore Proxmox network config from backup (document where stored)
