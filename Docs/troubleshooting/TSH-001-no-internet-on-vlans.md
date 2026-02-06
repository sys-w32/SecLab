# TSH-001: VLAN gets DHCP but no internet

Symptoms:
- Client receives IP/gateway/DNS
- Cannot browse internet / `curl` fails
- Sometimes ping works inconsistently

Most common root causes:
1) Outbound NAT missing for the VLAN subnet
2) DNS blocked (client cannot reach resolver)
3) Firewall rule order/alias mismatch (rule not actually matching traffic)
4) VLAN tagging mismatch (client on wrong VLAN)

Triage checklist (fast):
1) From client:
- ping gateway
- `nslookup example.com`
- `curl -I https://example.com`

2) On pfSense:
- firewall logs on that VLAN interface (what is being blocked?)
- confirm NAT includes the VLAN subnet
- confirm DNS resolver/listener and rules allow DNS from VLAN_NET to pfSense

Fix patterns:
- Add/repair outbound NAT for that VLAN
- Add explicit DNS allow: VLAN_NET → This Firewall :53
- Ensure block-to-localnets is below required allow rules (or vice versa depending on policy)

Prevention:
- After creating any new VLAN: immediately validate DHCP → DNS → HTTPS → logs.

