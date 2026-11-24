# DHCP

## Situation

Service: dnsmasq  
Vlans:
    Clients:
        172.28.33.10-250
        routeur: 172.28.34.254
        dns: 172.28.34.53
        domain-name: blo.blois.sportludique.fr
    Mana:
        10.5.150.2-3


## Config DNSMASQ
```dnsmasq
# Disables DNS
port=0

log-dhcp
dhcp-authoritative
dhcp-option=option:dns-server,172.28.34.53

# Server vlan DHCP, only used for testing
dhcp-range=server,172.28.34.10,172.28.34.250,255.255.255.0,12h
dhcp-option=server,option:router,172.28.34.254

# Clients, they also need to get in lan domain
dhcp-range=clients,172.28.33.10,172.28.33.250,255.255.255.0,12h
dhcp-option=clients,option:router,172.28.33.254
dhcp-option=clients,option:domain-name,blo.blois.sportludique.fr

# Mana vlan, no router nor domain
dhcp-range=mana,10.5.150.2,10.5.150.3,255.255.255.0,12h
```

## DHCP Relay

Le relais se fait sur le switch HP:
```
>sys
]dhcp enable
]dhcp relay server-group 12 ip 172.28.34.67
]int vlan 261 
261]dhcp select relay
261]dhcp relay server-select 12
```
