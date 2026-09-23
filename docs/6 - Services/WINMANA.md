# WIN MANA

Ce serveur est le seul serveur en GUI, il sert à administrer les autres serveurs Windows  

## Réseau
Ce serveur a plusieurs IP, une dans le réseau serveurs (`172.28.34.51`) et une dans le réseau de management (`10.5.150.51`)  


## Config générale

### NOM
Le nom se change comme suit:  
Paramètres -> à propos de -> renommer -> BLO-WIN-GUI

## Domaine
Paramètres -> Système -> à propos -> paramètres avancés -> Nom -> Domaine

### RDP
Paramètres -> Paramètres de Bureau à distance -> Activer
Recherche -> Pare-Feu avancé -> Bureau à distance (les 3 options: TCP, TCP-in, UDP) -> étendue -> première liste -> Ajouter uniquement le VLAN de Management -> appliquer

#### Remmina
Gestionnaire de connexion à distance avec le protocole RDP
Cliquer sur "+" en haut à gauche de la page puis ajouter les informations nécéssaires:
 - Serveur: {Adresse IP du Serveur}  
 - Nom d'utilisateur  
 - Mot de passe  
 - Domaine  


## Gestion
Il faut maintenant ajouter l'AD pour le gérer, sur le dashboard:  
Ajouter d'autres serveurs à gérer > Rechercher maintenant > BLO-AD


