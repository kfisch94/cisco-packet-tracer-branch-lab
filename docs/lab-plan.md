# Branch network walkthrough

## Session one Read the network

1. Open Cisco Packet Tracer and note the installed version from Help > About or the equivalent About menu.
2. Use File > Open to load `packet-tracer/branch-network-working.pkt` from this project. The upstream starter is preserved separately.
3. If Packet Tracer reports a version-compatibility issue, record that message before changing files or versions.
4. Choose the Logical workspace and Realtime mode. Fit the whole topology on screen and capture the labels and any instructions placed on the canvas.
5. Read all visible requirements and notes. Capture them separately if necessary. Do not invent device counts, addresses, or interfaces from the lab title.
6. Record the router/switch models, names, port connections, VLANs, subnets, gateways, and intended destinations shown in the actual file. Distinguish supplied values from values still to be chosen.
7. Before configuring devices, predict one same-subnet path and one routed path. Explain which address a host will ARP for in each case.
8. Record the starting state and a few baseline tests. A failed test is a baseline result, not a reason to change several settings at once.

First evidence: `01-starting-topology.png` and a Packet Tracer version record. No completed result is assumed from downloading a starter file.

## Session two Configure and verify switching

Review switch CLI modes, interface naming, access ports, VLAN membership, and trunks. Map each command to a requirement before applying it. Commands such as `show vlan brief` and `show interfaces trunk` can help inspect the current state where supported by the simulated model.

Check both ends of trunk links and verify host addressing. Capture a same-VLAN connectivity test with clear source and destination. Explain why VLAN separation creates distinct broadcast domains.

## Session three Add local routing

Use the actual router interfaces and VLAN IDs to build router-on-a-stick subinterfaces. Explain the 802.1Q tag, per-VLAN gateway, and the single physical router link. Check the parent interface and subinterfaces, then repeat tests between VLANs.

Do not blindly add `switchport trunk encapsulation dot1q`: some models have a fixed encapsulation and do not implement that command. Use model-supported commands and record the device models.

## Session four Connect the branches

Build the static routes required by the actual addressing plan. Check destination prefixes, next-hop reachability, and return paths. Use ping and the available traceroute/tracert tool to compare observed forwarding with the predicted path.

Use Simulation mode for a focused ARP and ICMP observation. Explain MAC addresses per hop, IP addresses end to end, and where VLAN tags are used. Packet Tracer is a simulation; observations do not establish physical throughput, cable quality, optical measurements, or full hardware feature support.

## Session five Troubleshoot

Save a known-working baseline after verification. Introduce one fault in a separate working file, such as incorrect access VLAN, a required VLAN missing from a trunk, a wrong gateway, or a missing return route. Choose faults supported by the actual topology.

For each fault: state the symptom, predict likely causes, inspect the relevant layer, apply a specific correction, repeat the original test, and record the result. Keep a small incident record rather than posting an unexplained wall of commands. Screenshot filename examples: `05-fault-symptom.png`, `06-diagnostic-output.png`, `07-restored-connectivity.png`.

## Session six Extend and publish

After the base exercise passes, consider DHCP and an access-control policy. Define exactly which source/destination/protocol combinations should pass or fail before writing ACLs. Cisco IOS ACLs are not automatically equivalent to pfSense's stateful rules; include the required return and infrastructure traffic in the policy and tests.

Export reviewed device configurations as text, save the final `.pkt`, and retain the verification matrix and fault records. Use lab-only credentials and review configs before publication. Credit the source topology and preserve its license. Publish only the results actually achieved.

Proposed repository name: `cisco-packet-tracer-branch-lab`.
