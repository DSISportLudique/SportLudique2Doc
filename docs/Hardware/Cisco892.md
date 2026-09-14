# Cisco 892 

## Reset

**Pendant la phase de boot, après avoir changer l'état de l'interupteur vers le 1.**
**Maintenir presser le bouton "Reset" à côté de l'interupteur.**

Il suffit d'attendre jusqu'à ce que le routeur engage ce qu'il appelle "System Configuration Dialog"
Entrer "no" quand demandé. Puis le routeur sera complètement reset.

## Out of the box config

ports 0-7: Gigabit LAN
port 8 et 9: Gigabit WAN

## Setup

Création du VLAN 150 et insertion du port 7 dans celui-ci
*VLAN 150 = VLAN de Management*

**Commandes:**

*Accès SSH*

- en  
- conf t  
- hostname BLO-ROU-892  
- username <Nom Admin> privilege 15 secret <password>  
- ip domain name BLO-ROU-892  
- crypto key generate rsa general-keys modulus 4096  
- ip ssh version 2  
- line vty 0 4  
- transport input ssh  
- login local  
- exit  

*Setup VLANs*

- Interface g7  
- switchport mode access    
- switchport access vlan 150  
- port-tagging  
- encapsulation dot1q 150  
- exit  

- Interface vlan 150  
- ip address X.X.X.X 255.255.255.0  

## .ssh/config

**/!\ Cette partie est à faire depuis le client SSH et doit être fait dans le fichier ~/.ssh/config /!\**
```
Host R253
    HostName X.X.X.X
    User <Nom Admin>
    KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
    HostkeyAlgorithms +ssh-rsa
    Ciphers aes128-cbc,3des-cbc
```
