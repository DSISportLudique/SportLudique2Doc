# Documentation Switch HP A5500

Relations ports/vlans  

## VLAN Management (150)

Interface GigabitEthernet 1/0/29
Interface GigabitEthernet 1/0/30
Interface GigabitEthernet 1/0/31
Interface GigabitEthernet 1/0/32

## VLAN Management (150)

Interface GigabitEthernet 1/0/29  
Interface GigabitEthernet 1/0/30  
Interface GigabitEthernet 1/0/31  
Interface GigabitEthernet 1/0/32  

## VLAN INTERCO (269)
Interface GigabitEthernet 1/0/2  
Interface GigabitEthernet 1/0/4  

## VLAN FAI1 (105)
Interface GigabitEthernet 2/0/1  
Interface GigabitEthernet 2/0/3  

## Ports Switch à Switch (105, 205)
Interface GigabitEthernet 2/0/2  
Interface GigabitEthernet 2/0/4  

## VLAN Client (261)
Interface GigabitEthernet 2/0/5  
Interface GigabitEthernet 2/0/6  

## Accès Internet

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
