# Lab 3 — Secured WAN: PPP/CHAP, DHCP relay & snooping, RIP

## Objective
Connect two remote LANs over a serial WAN and protect the exchange against rogue DHCP and man-in-the-middle attacks.

## Components
| Technology | Role |
|---|---|
| **PPP encapsulation + CHAP** | Authenticated point-to-point link; mutual router authentication |
| **DHCP server** (ISP-side router) | Pools `sw1` and `sw2`, with excluded addresses; simulates an ISP serving a company with no local DHCP |
| **DHCP relay** (`ip helper-address`) | Lets clients reach the remote DHCP server across routers |
| **DHCP snooping** | Rejects DHCP replies from untrusted ports |
| **RIP** | Dynamic routing between the sites |

A second company site (router R2) hosts its own DHCP: a Windows Server 2019 VM in VMware Workstation, bridged to the physical network (`vmnet0`). A **DHCP attacker** was added to test snooping.

## Tests
| Test | Result |
|---|---|
| PC1 gets its address from pool `sw1`, PC2 from pool `sw2` | Success |
| Ping between PC1 and PC2 across the WAN | Success — PPP, RIP and DHCP relay work |
| **Snooping test** (Packet Tracer): automatic addressing enabled on the PCs, R2's F0/1 shut down, then brought back up | With the interface down the PC did not obtain a configuration from the DHCP attacker; after restoring it, the PC received its configuration — snooping behaved as expected |

## Issues met
- On the **real hardware** the DHCP server in the VM did not hand out leases through the VMware bridge, so static addresses were used to complete the connectivity tests
- The **DHCP snooping** test was therefore done in **Packet Tracer** only

## Files
- `configs/` — ISP_DHCP, main router, R2, switches *(to add)*
- `../../cheatsheet/cisco-ios-quick-reference.md` — DHCP, relay, RIP and PPP command patterns
