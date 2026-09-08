# SSH Setup Switch HP A5500

## Création du VLAN de Management (VLAN 150)

*VLAN défini dans le cahier des charges*
```
en system-view:

vlan 150
quit
```

## Insertion du port de choisit dans le VLAN de Management

*Adapter vos ports à vos besoins*
```
interface GigabitEthernet1/0/32
port access vlan 150
ssh server enable
interface GigabitEthernet1/0/31
port access vlan 150
```

## Adresse IP du VLAN

*Adresse IP cachée*
```
interface Vlan-Interface 150
ip address X.X.X.X 255.255.255.0
```

## Création des utilisateurs administrateurs du switch

```
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

## Finition

```
user-interface vty 0 15
authentication-mode scheme
return

save safely force
```
