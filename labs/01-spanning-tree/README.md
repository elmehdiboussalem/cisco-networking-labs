# Lab 1 — Loop prevention with Spanning Tree (STP)

## Objective
Understand how STP removes switching loops in a redundant Ethernet topology while keeping the redundancy available for failover.

## Topology
Four switches (three Catalyst 2960 and one Catalyst 2950) in a redundant mesh, built first in Packet Tracer and then on real hardware. PCs on the `10.0.0.0/8` network are attached to access ports.

## Concepts applied
- Root bridge election (lowest bridge ID), root ports, designated ports, blocked ports
- BPDU exchange and continuous topology monitoring
- **PortFast** on end-user ports (skip listening / learning)
- **BPDU Guard** to shut down an access port that receives a BPDU
- Root bridge control with `spanning-tree vlan 1 root primary|secondary` and priority tuning

## Method
1. Build and validate the topology in Packet Tracer
2. Reproduce on physical switches (console access with PuTTY)
3. Configure each switch (root, priorities, PortFast, BPDU Guard)
4. Run the connectivity tests

> `errdisable recovery cause bpduguard` is optional and not supported on every switch model.

## Tests
| Test | Action | Result |
|---|---|---|
| 1 | PC on Switch 2 (Fa0/4) pings PC on Switch 1 (Fa0/4) | Success |
| 2 | Unplug the cable between Switch 4 and Switch 2, repeat the ping | Success — traffic reconverges through the remaining path |

**Conclusion:** STP correctly prevented loops and kept the network available after a link failure.

## Files
- `configs/` — one `show running-config` export per switch *(to add)*
- `../../screenshots/` — Packet Tracer topology and photo of the real setup *(to add)*
