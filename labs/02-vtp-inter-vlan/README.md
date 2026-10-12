# Lab 2 — VTP and inter-VLAN routing

## Objective
Centralize VLAN management with **VTP** and let hosts in different VLANs communicate through a router.

## Concepts applied
- **VTP** modes: server (creates / deletes VLANs and advertises them), client (synchronizes from the server), domain names
- **802.1Q** trunks between switches and towards the router
- **Router-on-a-stick**: one sub-interface per VLAN with `encapsulation dot1Q`

## Scenario A — Single VTP domain (Catalyst 2950)
- Switch **server**: creates the VLANs, builds trunks, assigns access ports
- Switch **client**: joins the VTP domain, trunk, port assignment
- Router: sub-interfaces for each VLAN

| Test | Result |
|---|---|
| PC in VLAN 2 (server side) ↔ PC in VLAN 3 (client side) | Ping works both ways |
| PC in VLAN 4 (server) ↔ PC in VLAN 2 (client) | Ping works both ways |
| PC in VLAN 3 (server) ↔ PC in VLAN 4 (client) | Ping works both ways |

## Scenario B — Two VTP domains (Catalyst 2960)
Two domains (`RT1` and `RT2`), each with its own server and its own VLANs, plus a **central switch acting as a client** that can be attached to either domain with `vtp domain RT1` / `vtp domain RT2`. When the domain changes, the switch takes its VLAN database from the new domain's server.

Observations:
- After moving the client to another domain, **port states changed** because the VLAN database changed
- Pings between devices in **different domains do not pass**

## Issues met
Changing the domain between RT1 and RT2 was laborious and did not always work on the first attempt, and replacing the central switch added difficulty — both showed how tightly VTP, VLAN databases and trunks interact.

## Files
- `configs/` — server, client and router configs *(to add)*
- `../../screenshots/` — Packet Tracer and hardware setups *(to add)*
