# VLANs

## VLANs à ajouter:
- 260-269  
- 105  
- 205   
- 150  

## Ports:  
- 2/0/1 & 2/0/3: LACP inter switch  
- 2/0/2 & 2/0/4: Ports WAN des routeurs  
- 1/0/1 & 1/0/3: Ports LAN des routeurs  
- 2/0/5 & 2/0/6: Ports Clients  

### VLANs + PORTS
- LACP: tous
- WAN: 105 || 205
- LAN: tous
- Client: 261 only

## Config (HP)

### VLANs
Pour chaques VLANs:  
```
] vlan XXX
vlan XXX] name Clients 
vlan XXX] quit 
```

### Trunk
Pour une interface X/X/X, ajouter tout les vlan (et 150 suivant le port):  
```
] interface Gi X/X/X
X/X/X] port link-type trunk
X/X/X] port trunk permit vlan [150] 105 205 260 to 269
X/X/X] undo shutdown
X/X/X] quit
```

### Access
Pour une interface X/X/X ayant acces au vlan X:  
```
] interface Gi X/X/X
X/X/X] port access vlan X
X/X/X] undo shutdown
X/X/X] quit
```
