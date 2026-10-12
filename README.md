# Cisco Networking Labs on Real Hardware

Four enterprise-networking labs first designed and validated in **Cisco Packet Tracer**, then **rebuilt on physical Cisco switches and routers**: Spanning Tree, VTP with inter-VLAN routing, a secured WAN (PPP/CHAP, DHCP relay, DHCP snooping, RIP), and a multi-protocol topology with HSRP, EIGRP, OSPF and NAT.

> Final-year project — DUT Networks & Telecommunications, EST Fès (2023–2024).

## Why hardware, not just simulation

A simulator accepts almost any command. Real devices expose what textbooks leave out: IOS feature differences between models, cabling, console access, ports that stay down, protocols that are not supported on older images. Documenting those differences was part of the project.

## Labs

| # | Lab | Technologies | Hardware validation |
|---|---|---|---|
| 1 | [Loop prevention](labs/01-spanning-tree/) | STP, root bridge election, PortFast, BPDU Guard | Failover tested by pulling a cable |
| 2 | [VLANs, VTP and inter-VLAN routing](labs/02-vtp-inter-vlan/) | VTP server/client, 802.1Q trunks, router-on-a-stick, two VTP domains | Cross-VLAN pings; domain change behaviour observed |
| 3 | [Secured WAN](labs/03-secure-wan/) | PPP + CHAP, DHCP server, DHCP relay (`ip helper-address`), DHCP snooping, RIP | PPP / RIP / relay validated; snooping validated in Packet Tracer |
| 4 | [Multi-protocol topology](labs/04-multi-protocol/) | HSRP, EIGRP, OSPF, NAT overload with pool, ACLs, VLANs | Gateway failover validated; OSPF not available on the hardware |

## Equipment

Catalyst **2960** (×4) and **2950** (×1) switches; routers from the **1600, 1700, 2600 and 2800** series; serial, console and Ethernet cabling; static switches and a hub for side tests. Tools: Packet Tracer, GNS3, PuTTY (console), Wireshark, Nmap.

## Quick reference

[`cheatsheet/cisco-ios-quick-reference.md`](cheatsheet/cisco-ios-quick-reference.md) collects the IOS commands used across the labs (interfaces, passwords, RIP / EIGRP / OSPF, ACLs, VLANs, VTP, STP protections, DHCP, relay, NAT / PAT, PPP PAP and CHAP).

## Repository structure

```
labs/
  01-spanning-tree/
  02-vtp-inter-vlan/
  03-secure-wan/
  04-multi-protocol/
cheatsheet/        IOS command quick reference
docs/              equipment and method
screenshots/       Packet Tracer topologies and photos of the real setups
configs/           exported running-configs per lab and device
```

Each lab folder has its own README (objective, topology, method, tests, issues met). The `show running-config` exports are still to be added — see [`configs/TO_ADD.md`](configs/TO_ADD.md).

## Lessons learned

- Always design and validate in a simulator first, then reproduce on hardware
- Hardware limits matter: the 2800-series routers used here did not support OSPF on the interface, so EIGRP carried the real-world test
- Changing VTP domains on a live switch has side effects on port states — plan it
- A DHCP server running in a VM on a bridged virtual switch did not hand out leases on the physical network: static addressing was used to finish the tests, and DHCP snooping was validated in Packet Tracer instead

## Author

El Mehdi Boussalem · Amine Elarouji — supervised by Pr. Jamal Belkadid (EST Fès).

## Sources

CCNA 200-301 Cert Guide (Vol. 1 and 2), CCNA course by Mr. Jamal Belkadid, howtonetwork.com, Cisco product documentation.
