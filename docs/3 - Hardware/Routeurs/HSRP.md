# HSRP

## Routeur Cisco 1921

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
