# Device configuration exports

SW1.txt, SW2.txt, R1.txt, and R2.txt are the actual supplied `show running-config` transcripts, retained without rewriting device settings. Reviewed against the addressing plan and screenshot results on 2026-10-25. All four include the final `end` marker.

Both switches have the expected access assignments and trunk allowed list. Both routers have the expected WAN address, three tagged VLAN gateways, and three remote static routes. R2 includes the restored route to 192.168.20.0/24.

## Rebuilding the switch VLAN database

These switch running-config outputs do not include VLAN creation/name entries. The screenshot evidence confirms VLANs 10, 20, and 30 exist with the expected names. When rebuilding from text on a fresh switch, create those VLANs explicitly on both switches:

```text
vlan 10
 name Management
vlan 20
 name Sales
vlan 30
 name HR
```

The files include CLI prompt/output headers and are review transcripts, not unattended deployment scripts. `Sw2` is the exported hostname spelling for the device labeled SW2 in the topology. IOS version lines in these transcripts describe the simulated devices, not the Packet Tracer application version.
