# RB-007: Backups and restore

Purpose: avoid rebuild-from-scratch events.

## What to back up
- pfSense config export (encrypted/off-repo)
- UniFi controller backup
- Wazuh configuration + any custom rules/decoders
- Proxmox VM backups/snapshots for critical VMs

## Restore drill (quarterly recommended)
- restore pfSense config into a fresh VM (test only)
- verify VLANs/NAT basic connectivity
- restore UniFi backup and confirm devices appear
