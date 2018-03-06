---
layout: post
title:  "Hopping from ipv4 to ipv6"
date:   2016-10-06 20:00:00 +0200
categories: vps ssh ipv4 ipv6
---
# SSH Configuration for Ipv6 Hop

Hosts with only an ipv4 address cannot connect to hosts with only a ipv6 address. That's unfortunate, because nowadays, you can cheaply rent VPS's with only ipv6 addresses.

When renting such VPS, I assumed that I had ipv6 enabled. I quickly learned that most ISP's in the Netherlands do not support ipv6 yet. Hopping through another host that had ipv6 enabled, seemed to be a good option. Luckily, I also had rented a VPS that had both ipv4 and ipv6.


For easy hopping, I configured the following in ~/.ssh/config:  


```
Host vpsipv6
   HostName xxxx:xxxx:xxxx::::xxxx:xxxx
   User root
   Port 22
   ProxyCommand ssh -4 root@hophost nc6 -6 %h %p
```



So from now on, I can connect to my ipv6 vps with:
```
ssh vpsipv6
```
