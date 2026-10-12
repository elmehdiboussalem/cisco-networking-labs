# Method

For every lab:

1. **Design** the topology and addressing on paper
2. **Simulate** in Cisco Packet Tracer and validate every test case
3. **Rebuild on physical equipment**: racking, cabling (console, Ethernet, serial DTE/DCE), console access with PuTTY
4. **Re-run the same tests** and compare with the simulator
5. **Document the differences** (unsupported features, port behaviour, model-specific commands)

## Tools
| Tool | Use |
|---|---|
| Cisco Packet Tracer | Simulation, DHCP snooping validation |
| GNS3 | Larger topologies |
| PuTTY | Console and Telnet / SSH access to devices |
| Wireshark | Packet capture and protocol analysis |
| Nmap | Discovery and service scanning |

## Bonus: crossover RJ45 cable
The project also covers building a crossover cable: cut and strip the UTP cable, untwist the pairs, arrange them in T568B order on one end and the crossed order on the other, insert, crimp and test with a cable tester.
