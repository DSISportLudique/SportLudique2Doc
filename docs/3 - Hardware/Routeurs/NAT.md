# NAT

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  

## Routeur Cisco 1921

Définir les interfaces de NAT (outside: WAN = Gi0/0.105 (vlan outside); inside: LAN = (Gi0/0.X))  

```
(config)# int Gi0/1.105
0/1.105# ip nat outside 
0/1.105# exit 
(config)# int Gi0/0.x
0/0.x# ip nat inside
0/0.x# exit
(config)# access-list 10 permit 192.168.69.0 0.0.0.255
(config)# access-list 10 permit 172.28.32.0 0.0.31.255
(config)# ip nat inside source list 10 interface Gi 0/1.105 overload
```

## Routeur Cisco 892

Définir les interfaces de NAT (outside: WAN = G9 (NAT outside); inside: LAN = G8 (NAT inside))  

```
(config)# int G9
G9# ip nat outside 
G9# exit 
(config)# int G8
G8# ip nat inside
G8# exit
(config)# access-list 10 permit 192.168.69.0 0.0.0.255
(config)# access-list 10 permit 172.28.32.0 0.0.31.255
(config)# ip nat inside source list 10 interface G9 overload
```
