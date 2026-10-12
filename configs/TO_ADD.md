# Configurations to add

The original report presents the command blocks as screenshots, so no device exports could be taken from the text. For each lab, connect to every device and run:

```
show running-config
```

Save under `configs/<lab>/<device>.txt`, for example:

```
configs/
  01-spanning-tree/switch1.txt ... switch4.txt
  02-vtp-inter-vlan/switch-server.txt, switch-client.txt, router.txt
  03-secure-wan/isp-dhcp.txt, router-principal.txt, r2.txt
  04-multi-protocol/external-router.txt, router1.txt, router2.txt, switch.txt
```

**Before committing:** replace values after `password`, `secret`, `username ... password` with placeholders (`<PASSWORD>`), and remove anything that identifies a real network.
