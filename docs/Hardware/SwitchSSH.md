#SSH Setup Switch HP A5500:

```
en system-view:

vlan 150
quit

interface GigabitEthernet1/0/32
port access vlan 150
ssh server enable

interface Vlan-Interface 150
ip address 10.5.150.253 255.255.255.0

local-user M3Enzo
authorization-attribute level 3
service-type ssh
password cipher <password>
quit

local-user M3lucas
authorization-attribute level 3
service-type ssh
password cipher <password>
quit
```
