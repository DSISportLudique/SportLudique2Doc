# OPNSense

## Création de la VM
- 8G de Stockage et 4G de RAM est assez  
- Rajouter une interface pour le WAN  (Ici la première était la "LAN" et donc l'interface "0" est la "LAN" et la "1" est la "WAN")  
- Sur Proxmox, mettre une interface pour commencer, définir si c'est la WAN ou la LAN  

## Setup
- Créer un ZFS pour le disque, pas de RAID car disque petit  
- Entrer le mot de passe du compte Root

## Log In
- entrer: "User: installer; password:Opnsense" à noter qu'au premier boot l'iso est en qwerty  
- Faire l'installation normalement  

## Options à faire en priorité

- 1) Assign Interfaces  
- 2) Set Interface IP Address  

#### 1) Assign Interfaces

Bien assigner l'interface WAN de proxmox à l'interface WAN de OPNSense et pareil pour le LAN

Attention: _les noms d'interfaces sont changées; c'est pourquoi il vaut mieux ne mettre qu'une seule interface, définir que c'est l'interface LAN ou WAN et après rajouter une autre interface pour bien se repérer et bien assigner_

(Une interface de Management sera rajoutée plus tard)

#### 2) Set Interface IP Address

- Associer une interface à une adresse IP  
- la gateway est mise sur le WAN  
- ne pas mettre d'IPV6  

**Ne pas mettre l'interface web en HTTP**

## Accès à l'interface Web de OPNSense

- https://<AdresseIpDuFirewall>/  
- entrer les logins root, seul accès à l'interface actuellement  
- Une fois sur l'interface; pour ajouter des users: System -> Access -> Users -> + -> mettre les droits admin au user  

## Accès à l'interface Web réservé au VLAN de Management

- Dans Proxmox, aller dans la configuration de la VM partie "hardware" et ajouter une carte réseau  
- Mettre cette carte réseau dans le VLAN de Management
- Retourner sur l'interface Web de OPNSense
- Mettre une adresse IP sur l'interface de Management dans "Interfaces" -> "[Mana]"
- Aller dans "Firewall" puis "Rules" et ajouter une règle permettant au VLAN de Management d'accéder à l'interface web
- Appliquer la règle
- Dans la partie "Firewall" -> "Settings" -> "Advanced": décocher "Disable administration anti-lockout rule"
- Créer une règle empêchant l'accès à l'interface Web de toutes les autres interfaces (LAN/WAN)
- Appliquer la règle puis passer par le VLAN de Management
