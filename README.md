# Cisco Packet Tracer Branch Network Lab

Built and tested a two-site network using VLANs, 802.1Q trunks, router-on-a-stick, and static routes. Then worked through three controlled faults to practice finding where connectivity breaks and verifying the fix.

**Status:** baseline and three fault/recovery exercises verified through CLI screenshots. Corrected device configurations saved on SW1, SW2, R1, and R2, as confirmed during the lab. Full running-configuration exports have been reviewed and included. Built with Cisco Packet Tracer 9.0.1.0858. The saved-project reopen check passed: PC1 to PC3 returned 4/4 replies with 0% loss.

## What I worked on

- Assigned Sales, HR, and Management endpoints to VLANs 20, 30, and 10 at each site.
- Configured trunks and router subinterfaces to route between local VLANs.
- Connected the sites over a /30 WAN and added three static routes on each router.
- Checked gateway reachability, local and remote traffic, routing tables, and the actual packet path.
- Introduced and corrected an access-VLAN mismatch, a missing trunk VLAN, and a missing return route.

![Working topology](evidence/08-working-topology.png)

The starter topology is by PPechmann. My contribution is the configuration, testing, and documented troubleshooting. This is a Packet Tracer simulation; the screenshots show simulated results.

## Read the lab

- [Addressing and port map](docs/addressing-plan.md)
- [Verification results](docs/verification.md)
- [Troubleshooting cases](docs/troubleshooting.md)
- [Original staged plan](docs/lab-plan.md)

## Files

- `packet-tracer/branch-network-baseline.pkt`: saved baseline checkpoint.
- `packet-tracer/branch-network-troubleshooting.pkt`: completed fault-exercise project with corrected configuration and a successful saved-project connectivity retest.
- `packet-tracer/branch-network-working.pkt`: earlier working copy.
- `evidence/`: captured configuration, test, failure, and recovery screenshots.
- [`configs/`](configs/README.md): reviewed, actual running-config transcripts for all four devices, with a VLAN database rebuild note.
- `upstream/`: original starter, MIT license, and source manifest.

DHCP and ACLs were not implemented in this exercise. VLAN membership separates local broadcast domains; the routers currently allow the tested inter-VLAN traffic.

## Getting used to the Cisco CLI

The first lesson was figuring out which mode I was in. `enable` gets me to privileged EXEC mode, then `configure terminal` opens configuration mode, and selecting an interface puts me in `(config-if)#`. Apparently one prompt was not enough.

The question mark became a useful shortcut: `?` shows the commands available in the current context. Checking the prompt first also explained why a command that worked a minute ago suddenly came back as invalid.

Then there was bringing up a router interface. The command is `no shutdown`. Fair enough: it removes the shutdown setting. Still, my first instinct was to try `enable`. Seeing the link turn green was satisfying, but this lab made the next lesson clear: a green link does not prove the VLAN, gateway, or return route is right.

Cisco is common enough in networking that getting comfortable with these modes is useful. After a few rounds of configuring, breaking, and fixing the network, the prompts started to make more sense.

## Source and credit

[PPechmann Packet Tracer Practice Labs](https://github.com/PPechmann/Packet-Tracer-Practice-Labs). Copyright (c) 2025 PPechmann; original MIT license retained in `upstream/LICENSE`. The original solution file was not consulted.
