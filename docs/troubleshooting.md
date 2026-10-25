# Controlled troubleshooting exercises

Each fault was deliberately introduced in the lab, investigated, corrected, and retested. These are practice scenarios rather than production incidents.

## 1. Workstation in the wrong access VLAN

PC1 retained 192.168.20.10/24 with gateway 192.168.20.1, but SW1 Fa0/1 was moved to VLAN 30. Its gateway ping failed with 100% loss. Comparing `ipconfig`, `show interfaces fastEthernet 0/1 switchport`, and `show vlan brief` exposed the mismatch: the endpoint belonged to Sales while its port belonged to HR.

Restored Fa0/1 with `switchport access vlan 20`. Gateway and remote PC3 tests then returned 4/4 replies each.

Evidence: [failure](../evidence/12-access-vlan-failure.png), [port diagnosis](../evidence/13-access-vlan-diagnosis.png), [correction](../evidence/14-access-vlan-correction.png), [retest](../evidence/15-access-vlan-recovery.png).

## 2. Sales VLAN missing from the trunk

Changed SW1 Gig0/1 allowed VLANs to 10,30. PC1 lost access to its gateway and PC3, even though Fa0/1 remained in VLAN 20. `show vlan brief` confirmed the access assignment; `show interfaces trunk` showed VLAN 20 missing from the uplink.

Server1 still reached Server2 with 4/4 replies, narrowing the issue to Sales rather than a complete WAN failure. An initial control ping was accidentally run on SW1; that result was excluded from the Server1 test because switch IP management had not been configured.

Restored the allowed list with `switchport trunk allowed vlan 10,20,30`. PC1 gateway and PC3 tests returned 4/4 each. The first forwarding output was transitional; a later capture showed all three VLANs forwarding.

Evidence: [failure](../evidence/16-trunk-vlan-failure.png), [diagnosis](../evidence/17-trunk-vlan-diagnosis.png), [unaffected Management test](../evidence/18-trunk-control-test.png), [retest](../evidence/19-trunk-recovery.png), [forwarding check](../evidence/20-trunk-forwarding-restored.png).

## 3. Missing return route

Removed R2's route to 192.168.20.0/24 via 10.0.0.1. PC1 still reached its local gateway with 4/4 replies, but its PC3 ping failed completely. Server1 -> Server2 continued to pass. R2's routing table lacked the Site A Sales route and had no default route.

The remaining forward route could carry the request toward PC3, but R2 had no route for replies to PC1's subnet. Restored `ip route 192.168.20.0 255.255.255.0 10.0.0.1`. PC1 -> PC3 returned 4/4 replies, and the trace again followed R1 -> R2 -> PC3.

Evidence: [missing route](../evidence/21-return-route-missing.png), [failure](../evidence/22-return-route-failure.png), [control test](../evidence/23-return-route-control-test.png), [restored route](../evidence/24-return-route-restored.png), [successful ping and trace](../evidence/25-return-route-recovery.png).

## Takeaway

Start with the endpoint address and gateway, then check access VLAN membership, the trunk, and routing in both directions. A working link does not establish that the correct VLAN or return route is present. Keep a known-good baseline and change one thing at a time.
