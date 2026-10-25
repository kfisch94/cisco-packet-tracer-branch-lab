# Observed topology and addressing plan

Source: topology and notes shown in the user's Packet Tracer screenshot, codex-clipboard-7a14c11b-9d2d-4ae9-a402-0da3c45c1999.png. The screenshot verifies the intended design, not completed device configuration. Packet Tracer version: 9.0.1.0858, verified from the About window.

## Devices and connections

- Site A: R1 (1941), SW1 (2960-24TT), PC1, PC2, Server1.
- Site B: R2 (1941), SW2 (2960-24TT), PC3, PC4, Server2.
- Each endpoint connects to its local switch. Each switch connects to its local router. R1 and R2 connect directly over the simulated WAN.
- Endpoint-to-switch links show green indicators. Router-facing links show red indicators in the baseline screenshot. This describes the starting screenshot. Subsequent captures confirm the configured links are up; see the verified port inventory below.

## VLAN membership and gateways

| VLAN | Role | Site A subnet | Site A gateway | Site A endpoint | Site B subnet | Site B gateway | Site B endpoint |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | Server1 | 192.168.110.0/24 | 192.168.110.1 | Server2 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 | PC1 | 192.168.120.0/24 | 192.168.120.1 | PC3 |
| 30 | HR | 192.168.30.0/24 | 192.168.30.1 | PC2 | 192.168.130.0/24 | 192.168.130.1 | PC4 |

All six endpoints were configured with host address .10 in their respective subnets (for example PC1 192.168.20.10 and Server2 192.168.110.10), using the gateways above. DNS was left unconfigured for IP-based testing. All listed /24 networks use 255.255.255.0.

WAN network: 10.0.0.0/30, mask 255.255.255.252. R1 is intended to use 10.0.0.1 and R2 10.0.0.2. Network address is .0 and broadcast is .3; these are not host assignments.

The same VLAN numbers at both sites identify local roles. They are separate subnets and Layer 2 domains, not a VLAN stretched over the routed WAN. PC1 to PC3 therefore needs a gateway and forward/return routes despite both being in local VLAN 20.

## Supplied task list

Create VLANs 10, 20, and 30 on both switches; assign endpoints; trunk each switch-to-router link; configure router-on-a-stick on both routers; apply the addressing plan; add static routes; and verify cross-site communication, including server-to-server and server-to-PC connectivity. The base requirements do not request ACL isolation between HR and Sales, so do not add restrictions before establishing the required baseline.

## Verified port inventory

| Link | Interfaces | Role |
| --- | --- | --- |
| SW1 to PC1 / PC2 / Server1 | Fa0/1 / Fa0/2 / Fa0/3 to endpoint Fa0 | Access VLANs 20 / 30 / 10 |
| SW2 to PC3 / PC4 / Server2 | Fa0/1 / Fa0/2 / Fa0/3 to endpoint Fa0 | Access VLANs 20 / 30 / 10 |
| SW1 to R1 | Gig0/1 to Gig0/1 | 802.1Q trunk; router subinterfaces .10, .20, .30 |
| SW2 to R2 | Gig0/1 to Gig0/1 | 802.1Q trunk; router subinterfaces .10, .20, .30 |
| R1 to R2 | Gig0/0 to Gig0/0 | 10.0.0.1/30 to 10.0.0.2/30 |
