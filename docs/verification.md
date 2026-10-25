# Verification results

Results below were observed in Packet Tracer screenshots during the guided exercise. A successful ping establishes ICMP reachability for that source/destination at that moment, not validation of every application or all possible host pairs.

| Check | Result | Evidence |
| --- | --- | --- |
| Both switch access-port assignments | Fa0/1 Sales 20; Fa0/2 HR 30; Fa0/3 Management 10 | 02, 03 |
| Both trunks | Gig0/1 using 802.1Q; VLANs 10,20,30 allowed and forwarding | 04, 05 |
| Static routing | Three remote /24 routes on each router via the opposite WAN IP | 06, 07 |
| PC1 to PC3 path | 192.168.20.1 -> 10.0.0.2 -> 192.168.120.10 | 09 |
| Server1 to Server2 and all four PCs | Each test 4/4 replies, 0% loss | 10 |
| Server2 to Server1 and all four PCs | Each test 4/4 replies, 0% loss | 11 |
| Access-VLAN correction | PC1 gateway and PC3 tests each 4/4 | 14, 15 |
| Trunk VLAN correction | PC1 gateway and PC3 tests each 4/4; all three VLANs forwarding | 19, 20 |
| Return-route correction | Route restored; PC1 to PC3 4/4 and expected three-hop trace | 24, 25 |

Evidence numbers refer to the numbered filenames in `../evidence/`.

Router subinterfaces were also observed up/up for all three VLANs at both sites. WAN R1 -> R2 ping returned 5/5; the initial R2 -> R1 run returned 4/5. The cause of that first lost reply was not independently captured; later endpoint tests passed.

## Save and export status

The operator confirmed saving device startup configurations on all four network devices after recovery. Baseline and troubleshooting .pkt files exist locally. After the requested save-and-reopen check, the supplied PC1 -> PC3 retest returned 4/4 replies with 0% loss (evidence/27-saved-project-retest.png). This verifies that tested path after reopening; it does not repeat every baseline test. All four supplied running-configuration exports were reviewed against the addressing plan: endpoint VLAN assignments, allowed trunk VLANs, WAN addresses, tagged subinterface gateways, and all six static routes match. The exports include the final end marker. Switch VLAN creation/name entries are absent from these transcripts; see configs/README.md for the rebuild note. Packet Tracer application version: 9.0.1.0858, verified in evidence/26-packet-tracer-version.png.
