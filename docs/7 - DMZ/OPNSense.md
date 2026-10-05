# OPNSense

## Creation de la VM
- 8G de Stockage et 4G de RAM est assez  
- Rajouter une interface pour le WAN  (Ici la premiere etait la "LAN" et donc l'interface "0" est la "LAN" et la "1" est la "WAN")  
- Sur Proxmox, mettre une interface pour commencer, definir si c'est la WAN ou la LAN  

## Setup
- Creer un ZFS pour le disque, pas de RAID car disque petit  
- Entrer le mot de passe du compte Root

## Log In
- entrer: "User: installer; password:Opnsense" a noter qu'au premier boot l'iso est en qwerty  
- Faire l'installation normalement  

## Options à faire en priorité
- 1) Assign Interfaces  
- 2) Set Interface IP Address  

#### 1) Assign Interfaces
Bien assigner l'interface WAN de proxmox à l'interface WAN de OPNSense et pareil pour le LAN

Attention: _les noms d'interfaces sont changees; c'est pourquoi il vaut mieux ne mettre qu'une seule interface, definir que c'est l'interface LAN ou WAN et apres rajouter une autre interface pour bien se reperer et bien assigner_

(Une interface de Management sera rajoutee plus tard)

#### 2) Set Interface IP Address
- Associer une interface à une adresse IP  
- la gateway est mise sur le WAN  
- ne pas mettre d'IPV6  

**Ne pas mettre l'interface web en HTTP**

<<<<<<< HEAD
## Acces a l'interface Web de OPNSense

- https://\<AdresseIpDuFirewall\>/  

=======
<<<<<<< HEAD
## Accès à l'interface Web de OPNSense
- https://<AdresseIpDuFirewall>/  
=======
## Acces a l'interface Web de OPNSense

- https://\<AdresseIpDuFirewall\>/  
>>>>>>> 593d648 (fixed ip address in opnsense doc)
>>>>>>> fbaf34b (fixed ip address in opnsense doc)
- entrer les logins root, seul acces a l'interface actuellement  
- Une fois sur l'interface; pour ajouter des users: System -> Access -> Users -> + -> mettre les droits admin au user  

## Accès à l'interface Web réservé au VLAN de Management
- Dans Proxmox, aller dans la configuration de la VM partie "hardware" et ajouter une carte réseau  
- Mettre cette carte réseau dans le VLAN de Management
- Retourner sur l'interface Web de OPNSense
- Mettre une adresse IP sur l'interface de Management dans "Interfaces" -> "[Mana]"
- Aller dans "Firewall" puis "Rules" et ajouter une regle permettant au VLAN de Management d'acceder a l'interface web
- Appliquer la regle
- Dans la partie "Firewall" -> "Settings" -> "Advanced": decocher "Disable administration anti-lockout rule"
- Creer une regle empÃªchant l'acces a l'interface Web de toutes les autres interfaces (LAN/WAN)
- Appliquer la regle puis passer par le VLAN de Management

