# Cisco 1921

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  


## Début

Avant de commencer a travailler, il faut reset le routeur, repartir sur une base propre

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

Création des VLANs: 150, 260-269, 205, 105
WAN: 802.1Q vlans: 105 + 205
	- GE 0/1  
    - ip: 221.87.141.1/30  
LAN: 802.1Q vlans: 150 260-269
	- GE 0/0  
	- ip: X.X.X.X/24  
	- ip: 192.168.69.254/24  
Routes:  
    - 0.0.0.0/0 -> 221.87.141.2/30  
    - 172.28.32.0/19 -> 221.87.141.2/30  


## Configuration

*La premiere etape est le setup de l'interface de management afin de pouvoir administrer notre routeur a distance*

### Interface de mana

```
> conf t
(config)# interface gi 0/0

(config)# interface gi 0/0.150
(config)# ip address X.X.X.X 255.255.255.0
(config)# encapsulation dot1q 150 native
(config)# no shut
```

### Activer SSH

```
(config)#ip domain name blo-rou.lol // change this ofc
(config)#username USER privilege 15 secret PASS
(config)#ip ssh version 2
(config)#crypto key generate rsa general-keys modulus 4096
(config)#line vty 0 4
(config-line)#transport input ssh
(config-line)#end

(config)#access-list 23 permit X.X.X.X 0.0.0.255
(config)#line vty 0 15
(config-line)#transport input ssh
(config-line)#access-class 23 in
(config-line)#exit
```

### Configuration du client SSH

Dans `~/.ssh/config`  
```
Host r1
	HostName X.X.X.X
	User USER 
	Port 22
	KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
	HostkeyAlgorithms +ssh-rsa
	Ciphers aes128-cbc,3des-cbc
```
Connexion par ssh r1  

### Enable password

```
enable secret 4 PASSWORD

*L'etape d'apres est de permettre un acces a internet*
*Donc il faut configurer l'interface qui va vers l'exterieur (WAN) puis celle qui donne sur notre reseau interne (LAN)*

### Interface WAN

#### IP
L'adresse donnée est 221.87.141.2/30  
Le seul autre hôte dispo est le 221.87.141.1/30, il sera donc notre ip WAN  
```

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

//MSG au lancement d'une console sur le router

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

La route suivante est nécessaire pour router les vlans

```
ip route 172.28.32.0 255.255.224.0 192.168.69.1 permanent
```
