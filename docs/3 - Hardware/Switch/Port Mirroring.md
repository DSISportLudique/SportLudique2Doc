# Setup Port mirroring Switch HP

## Définition

Le port mirroring permet de copier le traffic d'un ou plusieurs ports d'un Switch vers un autre port pour analyse des trames du réseau sans interruption.

## Mise en place sur un Switch HP A5500

```
HP>system-view
HP]mirroring-group 1 local
HP]interface GX/0/X
X/0/X]mirroring-group 1 monitor-port
X/0/X]quit
HP]interface GY/0/Y
Y/0/Y]mirroring-group 1 mirroring-port
Y/0/Y]quit
```

Utiliser ensuite un outil comme tcpdump ou un logiciel comme wireshark.
Se connecter en direct sur le port mis en monitor-port.
