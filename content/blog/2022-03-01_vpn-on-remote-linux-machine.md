+++
title = "vpn on remote linux machine"

[taxonomies]
tags = ["linux", "iproutes", "bash", "vpn"]
+++

```text
a.b.c.d    — public IP
a.b.c.0/24 — public IP subnet
x.x.x.1     — gateway
```

To find gateway: `$ ip route | grep default`

```
ip rule add table 128 from a.b.c.d`
ip route add table 128 to a.b.c.0/24 dev eth0
ip route add table 128 default via x.x.x.1
```

and make sure table 128 is first after loopback in the rules.

To check: `ip rule`

To change order: 
```
ip rule add pref YYY from a.b.c.d lookup 128
ip rule del pref XXX from a.b.c.d lookup 128
```
where 
* `XXX` is the current priority value 
* `YYY` > 0 && `YYY` < (first after loopback)



Sources:
* <https://linux.die.net/man/8/ip>