# Setup
[setup](https://github.com/DSISportLudique/Scripts/blob/main/setup)

## Description
Mets en place une vm apres clonage de la vm principale BLO-VM-TEMPLATE

## Utilisation
sudo setup IPSERVEUR GATEWAY IPMANA HOSTNAME  
NB: si HOSTNAME ne commence pas par BLO- le script l'ajoutera lui meme
    si IPSERVEUR est dans la DMZ, une route sera ajoutée pour assurer la connection du lan à elle
