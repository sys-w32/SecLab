# TSH-004: Discord voice issues from Guest Wi-Fi

Symptoms:
- Discord text works but voice fails or can’t connect to some channels

Observed fix pattern:
- Allowing port range 2083–2096 resolved voice connectivity in this environment, as well as ranges 35,000-65535 for paublic channels

Approach:
1) Confirm issue is reproducible on affected VLAN
2) Check firewall logs for blocked Discord-related flows
3) Add a narrowly scoped allow rule for the required port range
4) Re-test across multiple channels

Notes:
- Keep the rule as specific as possible (source VLAN, destination any/WAN, ports only).
- Document the rule and the rationale (so you can remove/refine later).
