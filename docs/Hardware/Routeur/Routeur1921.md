# Configuration Routeur Cisco 1921

## Reset le routeur
```
\> show version
recercher Configuration register is X
X ici est 0x2101
reboot le router, au boot: ctrl+pause pour entrer en rommon
rommon 1> confreg 0x2142
rommon 2> reset
\> en
\# write
reboot le router, au boot: ctrl+pause pour entrer en rommon
rommon 1> confreg 0x2101
rommon 2> reset
```

## Rappel de configuration 
Création vlan 150 260-269 200-205 100-105
WAN: 802.1Q  
	- GE 0/1  
LAN: 802.1Q tout    
	- GE 0/0  
	- ip: 10.5.150.254/24 (.254 pour chaques réseaux)  


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
