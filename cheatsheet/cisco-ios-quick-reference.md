# Cisco IOS quick reference

Command patterns used across the labs. Values in `<angle brackets>` are placeholders. Commands are grouped by device type: `Router#` or `Switch#` prompts are used where the command is device-specific.

## Basics

```
Router> enable
Router# configure terminal
Router(config)# hostname <NAME>
```

### Interfaces
```
Router(config)# interface FastEthernet <slot/port>
Router(config-if)# ip address <IP> <MASK>
Router(config-if)# no shutdown
Router(config-if)# exit

! Serial (DCE side needs a clock rate)
Router(config)# interface serial <slot/port>
Router(config-if)# ip address <IP> <MASK>
Router(config-if)# clock rate <RATE>
Router(config-if)# no shutdown
```

### Access passwords
```
Router(config)# line console 0
Router(config-line)# password <PASSWORD>
Router(config-line)# login

Router(config)# line vty 0 4
Router(config-line)# password <PASSWORD>
Router(config-line)# login

! Privileged mode: prefer "secret" (encrypted) over "password" (clear text)
Router(config)# enable secret <PASSWORD>
```

## Dynamic routing

```
! RIP
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network <NETWORK>

! EIGRP
Router(config)# router eigrp 1
Router(config-router)# network <NETWORK> <WILDCARD>

! OSPF
Router(config)# router ospf 1
Router(config-router)# network <NETWORK> <WILDCARD> area 0
```

## ACLs
```
Router(config)# ip access-list standard|extended <NAME|NUMBER>
Router(config-std-nacl)# permit|deny <SOURCE> <WILDCARD>
Router(config)# interface <TYPE> <NUMBER>
Router(config-if)# ip access-group <NAME|NUMBER> in|out
```

## VLANs, trunks, inter-VLAN routing

```
! Create VLANs
Switch(config)# vlan <ID>
Switch(config-vlan)# name <NAME>

! Access port
Switch(config)# interface fastethernet <slot/port>
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan <ID>

! Trunk
Switch(config)# interface fastethernet <slot/port>
Switch(config-if)# switchport mode trunk

! Router sub-interface (router-on-a-stick)
Router(config)# interface fastEthernet <slot/port>.<VLAN-ID>
Router(config-subif)# encapsulation dot1Q <VLAN-ID>
Router(config-subif)# ip address <IP> <MASK>
Router(config-subif)# no shutdown
```

## VTP
```
! Server
Switch(config)# vtp domain <DOMAIN>
Switch(config)# vtp mode server
Switch(config)# vlan <ID>
Switch(config-vlan)# name <NAME>

! Client
Switch(config)# vtp domain <DOMAIN>
Switch(config)# vtp mode client
```

## Spanning Tree
```
Switch(config)# spanning-tree vlan 1 root primary
Switch(config)# spanning-tree vlan 1 root secondary
Switch(config)# spanning-tree vlan 1 priority <0-61440>

Switch(config)# interface fastEthernet <slot/port>
Switch(config-if)# spanning-tree vlan 1 port-priority <0-240>

! PortFast and BPDU Guard
Switch(config)# spanning-tree portfast default
Switch(config)# spanning-tree portfast bpduguard default

Switch(config)# interface fastEthernet <slot/port>
Switch(config-if)# spanning-tree portfast
Switch(config-if)# spanning-tree bpduguard enable
```

## DHCP and relay
```
Router(config)# ip dhcp excluded-address <IP>
Router(config)# ip dhcp pool <POOL-NAME>
Router(dhcp-config)# network <NETWORK> <MASK>
Router(dhcp-config)# default-router <GATEWAY>
Router(dhcp-config)# dns-server <DNS>
Router(dhcp-config)# domain-name <DOMAIN>

! Relay agent: point clients' gateway interface to the remote DHCP server
Router(config)# interface fastEthernet <slot/port>
Router(config-if)# ip helper-address <DHCP-SERVER-IP>
```

## NAT / PAT
```
! Static NAT
Router(config)# ip nat inside source static <INSIDE-LOCAL> <INSIDE-GLOBAL>

! Dynamic NAT with a pool
Router(config)# ip nat pool <POOL> <FIRST-IP> <LAST-IP> netmask <MASK>
Router(config)# access-list <1-99> permit <NETWORK> <WILDCARD>
Router(config)# ip nat inside source list <1-99> pool <POOL>

! PAT (overload) on one public address
Router(config)# ip nat inside source list <1-99> interface serial <slot/port> overload

! PAT (overload) on a pool
Router(config)# ip nat inside source list <1-99> pool <POOL> overload

! Mark the interfaces
Router(config)# interface fastEthernet <slot/port>
Router(config-if)# ip nat inside
Router(config)# interface serial <slot/port>
Router(config-if)# ip nat outside

! Verify
Router# show ip nat translations
```

## HSRP
```
Router(config-if)# standby <GROUP> ip <VIRTUAL-IP>
Router(config-if)# standby <GROUP> priority <PRIORITY>
Router(config-if)# standby <GROUP> preempt
Router# show standby
```

## PPP
```
! PAP
Router(config)# username <PEER-ROUTER-NAME> password <PASSWORD>
Router(config)# interface serial <slot/port>
Router(config-if)# encapsulation ppp
Router(config-if)# ppp authentication pap
Router(config-if)# ppp pap sent-username <PEER-ROUTER-NAME> password <PASSWORD>

! CHAP (preferred: the password is never sent in clear text)
Router(config)# username <PEER-ROUTER-NAME> password <PASSWORD>
Router(config)# interface serial <slot/port>
Router(config-if)# encapsulation ppp
Router(config-if)# ppp authentication chap
```

## Verification habits
```
show running-config
show ip interface brief
show ip route
show vlan brief
show interfaces trunk
show spanning-tree
show vtp status
show standby
show ip dhcp binding
```
