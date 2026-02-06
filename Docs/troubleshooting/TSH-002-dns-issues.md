# TSH-002: DNS failures (common cause of “internet is down”)

Symptoms:
- ping 1.1.1.1 works, but websites don’t load
- `nslookup` times out or returns SERVFAIL

Checks:
- Client DNS server points where you expect (often pfSense interface IP)
- pfSense DNS resolver is enabled and listening on that interface
- Firewall rules allow DNS to pfSense on that VLAN
- No accidental “block DNS” rule above the allow

Fix:
- Add rule: VLAN_NET → This Firewall (TCP/UDP 53)
- Ensure DNS resolver is bound appropriately
- Re-test with `nslookup` and `curl`
