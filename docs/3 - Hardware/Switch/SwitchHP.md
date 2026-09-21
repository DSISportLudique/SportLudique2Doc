# Documentation Switch HP A5500

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  

## Relations ports/vlans  

| Port | VLAN(s) | Usage |
|------|---------|-------|
| Gi 2/0/1, 2/0/3 | tous | Inter-switch |
| Gi 2/0/2, 2/0/4 | 105 & 205 & 150 & 262 | WAN routeurs, Mana vers Proxmox et VLAN Serveurs pour Proxmox |
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
>system-view
]interface GigabitEthernet 2/0/2
2/0/2]port link-type trunk
2/0/2]port trunk permit vlan 105 205 150 262

]interface vlan 269
Vlan 269]ip address 192.168.69.1 255.255.255.0
Vlan 269]quit

]interface vlan 150
Vlan 150]ip address 10.5.150.1 255.255.255.0
Vlan 150]quit

]interface vlan 261
Vlan 261]ip address 172.28.33.254 255.255.255.0
Vlan 261]quit

]interface vlan 262
Vlan 262]ip address 172.28.34.254 255.255.255.0
Vlan 262]quit

]ip route-static 0.0.0.0 0.0.0.0 192.168.69.254 description default-route
```

(faites pour notre vlan client)
```
]vlan 261
Vlan 261]description Clients
Vlan 261]quit

]vlan 269
Vlan 269]description interconnexion
Vlan 269]quit

]vlan 262
Vlan 262]description Serveurs
Vlan 262]quit

]vlan 150
Vlan 150]description Management
Vlan 150]quit

1/0/2]interface GigabitEthernet 1/0/2  
1/0/2]port link-type trunk
1/0/2]port trunk vlan 269
1/0/2]quit

]interface vlan 261
Vlan 261]ip address 172.28.33.254 255.255.255.0
Vlan 261]quit
```

## Management du routeur

sur l'interface connectée au routeur, j'ajoute le vlan de management au trunk (150) et le vlan d'interconnexion

#### Commandes

```
>system-view
]interface GigabitEthernet 1/0/2
1/0/2]port trunk permit vlan 150 269
1/0/2]quit
```

(pvid = tags les frames non-taggés, c'est l'équivalent du VLAN natif
permet l'accès à l'interface de management du routeur [Source](https://community.hpe.com/t5/comware-based/dymanic-tagged-vlan-assingment-hp-5500/td-p/6763660)

## NB

Le switch n'apparait pas durant un traceroute, afin de changer ça:

```
>system-view
]ip ttl-expires enable
]ip unreachables enable
]quit
>save
```
