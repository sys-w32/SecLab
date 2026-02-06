# RB-003: pfSense firewall + NAT baseline (multi-VLAN)

Purpose: consistent baseline policy so VLANs have expected connectivity.

## Baseline rules (intent)
For each client VLAN:
- Pass: VLAN_NET → This Firewall (DNS 53/UDP+TCP) [if pfSense is resolver]
- Pass: VLAN_NET → WAN (80/443) as required
- Block: VLAN_NET → RFC1918 (or “LocalNets” alias) (prevents lateral movement)

Important: if pfSense UI prevents negation/alias patterns you want, define explicit aliases:
- LOCAL_NETS = 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12 (adjust to your environment)
- Then block “to LOCAL_NETS” in the right direction.

## NAT (common failure point)
- Confirm outbound NAT includes each VLAN interface.
- If using Automatic: verify it actually generated rules for new VLANs.
- If Manual/Hybrid: add explicit NAT mappings for VLAN subnets to WAN.

## Validation checklist (per VLAN)
1) DHCP works
2) ping gateway works
3) DNS resolves (`nslookup example.com`)
4) `curl -I https://example.com` works
5) pfSense firewall log shows expected passes/blocks
