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
- ip ssh version 2
- crypto key generate rsa general-keys modulus 4096
- line vty 0 4

*Setup VLANs*
- Interface g7
- port-tagging
- encapsulation dot1q 150
- set cos 6

- Interface vlan 150
- ip address X.X.X.X 255.255.255.0


