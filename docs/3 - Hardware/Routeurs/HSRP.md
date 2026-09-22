# HSRP

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  

## But
Gestion du *Fail-over*, empêche le SPOF de la passerelle unique (Single Point Of Failure) 


## Routeur Cisco 1921

### HSRP pour accès des Vlans Clients, Serveurs etc.
```
>en
#conf t
(config)#in g0/1.269
(config-subif)#description LAN
(config-subif)#ip address 192.168.69.253 255.255.255.0
(config-subif)#standby 1 ip 192.168.69.254
(config-subif)#standby 1 priority 150
(config-subif)#standby 1 preempt
(config-subif)#end
```

## Routeur Cisco 892

### HSRP pour accès des Vlans Clients, Serveurs etc.
(VLAN 269 = VLAN Interconnexion)

```
>en
#conf t
(config)#in g8.269
(config-subif)#description LAN
(config-subif)#ip address 192.168.69.252 255.255.255.0
(config-subif)#standby 1 ip 192.168.69.254
(config-subif)#standby 1 priority 110
(config-subif)#standby 1 preempt
(config-subif)#end
```

# IP SLA

## But
Tester la connectivité au routeur FAI distant afin de garantir un accès réseau, et switch de routeur dans le HSRP si ça ne passe pas

## Routeur Cisco 1921

IP SLA est bloquer par une licence sur le Cisco 1921.

Source: 'https://www.cisco.com/c/dam/en/us/td/docs/solutions/SBA/August2012/Cisco_SBA_BN_NetworkMonitoringUsingIPSLAAndPrimeLMSDeploymentGuide-Aug2012.pdf'

Citation: 'Cisco IOS IP Base—Cisco IP SLA responder is included in the IP Base image. Cisco IP SLA sender, for operations beyond basic Internet Control Message Protocol (ICMP) operations, requires an image beyond IP Base (for example, Unified Communications, Security, etc.).'


## Routeur Cisco 892

(VLAN 269 = VLAN Interconnexion)

```
>en
#conf t
(config)#ip sla 1
(config-ip-sla)#icmp-echo 183.44.41.1 source-interface GigabitEthernet9
(config-ip-sla)#frequency 5
(config-ip-sla)#exit
(config)#ip sla schedule 1 life forever start-time now
(config)#track 1 ip sla 1 reachability
(config)#interface GigabitEthernet8
(config-if)#standby 1 track 10 decrement 20
(config-if)#end
```

