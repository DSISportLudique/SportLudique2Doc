# Documentation Switch HP A5500

## Relations ports/vlans  

| Port | VLAN(s) | Usage |
|------|---------|-------|
| Gi 2/0/1, 2/0/3 | tous | Inter-switch |
| Gi 2/0/2, 2/0/4 | 105 & 205 | WAN routeurs |
| Gi 1/0/1, 1/0/3 | tous | LAN routeurs |
| Gi 2/0/5, 2/0/6 | 261 | Clients |
| Gi 1/0/2, 1/0/4 | 269 | Interco |
| Gi 1/0/29 à 1/0/32 | 150 | Management |

## Config

### Accès Internet

#### Logique

- mise en place du port trunk vers l'interface inside du routeur  
- trunk comprend le vlan d'interconnexion (269) ainsi que les autres vlans utiles  
- création de la route par défaut  

#### Commandes

(interface réseau interconnexion)
```
interface GigabitEthernet 2/0/2
port link-type trunk
port trunk permit vlan 105
exit

interface vlan 105
ip address 192.168.69.1 255.255.255.0

ip route-static 0.0.0.0 0.0.0.0 192.168.69.254 description default-route
```

(faites pour notre vlan client)
```
vlan 261
description Clients
exit

vlan 105
description FAI1

interface GigabitEthernet 1/0/2  
port link-type trunk
port trunk vlan 261
exit

interface vlan 261
ip address 172.28.33.254 255.255.255.0
exit
```

## Management du routeur

sur l'interface connectée au routeur, j'ajoute le vlan de management au trunk (150)

#### Commandes

```
interface GigabitEthernet 1/0/2
port trunk permit vlan 150
port trunk pvid vlan 150
```

(pvid = tags les frames non-taggés, c'est l'équivalent du VLAN natif
permet l'accès à l'interface de management du routeur
Source: https://community.hpe.com/t5/comware-based/dymanic-tagged-vlan-assingment-hp-5500/td-p/6763660)  

## NB
Le switch n'apparait pas durant un traceroute, afin de changer ça:

```
system-view
ip ttl-expires enable
ip unreachables enable
quit
save
```
