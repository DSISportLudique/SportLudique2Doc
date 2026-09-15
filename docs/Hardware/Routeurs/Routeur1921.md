# Cisco 1921

## Reset le routeur 
```
> show version
recercher Configuration register is X
X ici est 0x2101
reboot le router, au boot: ctrl+pause pour entrer en rommon
rommon 1> confreg 0x2142
rommon 2> reset
> en
# write
reboot le router, au boot: ctrl+pause pour entrer en rommon
rommon 1> confreg 0x2101
rommon 2> reset
```

## Rappel de configuration 
Création vlan 150 260-269 200-205 100-105
WAN: 802.1Q vlans: 105 + 205
	- GE 0/1  
    - ip: 221.87.141.1/30
LAN: 802.1Q vlans: tout -105 -205   
	- GE 0/0  
	- ip: 10.5.150.254/24 (.254 pour chaques réseaux)  
	- ip: 192.168.69.254/24  
Routes:  
    - 0.0.0.0/0 -> 221.87.141.2/30  
    - 172.28.32.0/19 -> 221.87.141.2/30


## Configuration
### Interface de mana
```
> conf t
(config)# interface gi 0/0

(config)# interface gi 0/0.150
(config)# ip adress 10.5.150.254 255.255.255.0
(config)# ecapsulation dot1q 150 native
(config)# no shut
```

### Interface WAN

#### IP
L'adresse donnée est 221.87.141.2/30  
Le seul autre hôte dispo est le 221.87.141.1/30, il sera donc notre ip WAN  

#### Conf
```
(config)# interface gi 0/1.105
0/1.105# encapsulation dot1q 105 native
0/1.105# ip address 221.87.141.1 255.255.255.252
0/1.105# no shut
```

### Interface LAN

#### IP 

L'interface LAN du routeur est dans le réseau d'interco (vlan 269, 192.168.69.0/24), sont ip sera 192.168.69.254  
#### Conf
Pour le vlan X:  
```
(config)# interface Gi0/0.X
0/0.X# encapsulation dot1q X
0/0.X# ip address <ip> <masque>
0/0.X# no shut
```

### Enable password
```
enable secret 4 PASSWORD
```

//MSG au lancement d'une console sur le router

### Activer SSH
```
(config)#ip domain name blo-rou.lol // change this ofc
(config)#username USER privilege 15 secret PASS
(config)#ip ssh version 2
(config)#crypto key generate rsa general-keys modulus 4096
(config)#line vty 0 4
(config-line)#transport input ssh
(config-line)#end

(config)#access-list 23 permit 10.5.150.0 0.0.0.255
(config)#line vty 0 15
(config-line)#transport input ssh
(config-line)#access-class 23 in
(config-line)#exit
```

### Configuration du client SSH
Dans `~/.ssh/config`  
```
Host r1
	HostName 10.5.150.254
	User USER 
	Port 22
	KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
	HostkeyAlgorithms +ssh-rsa
	Ciphers aes128-cbc,3des-cbc
```
Connexion par ssh r1  

### NAT
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

### Routes
La passerelle par défaut est, comme définie plus haut, 221.87.141.2/30
```
ip route 0.0.0.0 0.0.0.0 221.87.141.2 permanent
```
La route suivante est nécessaire pour routé les vlans
```
ip route 172.28.32.0 255.255.224.0 192.168.69.1 permanent
```
