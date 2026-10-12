# Lab 4 — Multi-protocol topology: HSRP, EIGRP, OSPF, NAT

## Problem
How can a company reach external networks efficiently while keeping its **default gateways redundant** and its internal addressing hidden?

## Solution
- **VLANs** for the internal hosts and **HSRP** between two routers, with one active gateway per VLAN so traffic is shared
- **EIGRP** and **OSPF** towards the external router
- **NAT with a pool and overload (PAT)** controlled by an **ACL**

## Design steps
**HSRP:** pick the interfaces, a group number per interface, the virtual IP, the router priority, enable preemption, repeat on the peer, verify with `show standby`.

**NAT with pool + overload:** define the pool, mark inside/outside interfaces, permit the internal networks in an ACL, bind the ACL to the pool with `overload`, verify with `show ip nat translations`.

**OSPF:** place the Fast Ethernet and loopback interfaces in area 0.

## Packet Tracer vs real hardware
The real routers (2800 series) **did not support OSPF on the interface**, so on hardware the lab runs on **EIGRP only** (an extra `network 192.168.2.0 0.0.0.255` statement was added on the external router). OSPF was validated in Packet Tracer.

## Tests
| Test | Result |
|---|---|
| PC1 and PC2 ping an external address (8.8.8.8) | Success |
| Check which gateway each PC uses | PC1 and PC2 use **different** gateway routers → load shared |
| Simulate a failed gateway router and send traffic from the VLAN 20 PC to the external network | **HSRP failover works** |

## Files
- `configs/` — External_Router, router_1, router_2, switch *(to add)*
