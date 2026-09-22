# Switch LACP

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  

## Introduction
Le LACP est une méthode d'aggrégation dynamique de port (l'aggrégation statique requiert de configurer les ports sur les 2 équipements)  

### Source
Documentation trouvée [ici](https://sym-0ne.github.io/sport-ludique-Chartres/SWCore/lacp_hp_a5500/)


## Configuration du LACP

Création de l'interface aggrégée
```
] int brige-aggregation 1
brige-aggregation1] link-aggregation mode dynamic
```

Bind les interfaces physique à l'interface aggrégée (les interfaces aggrégées sont ici 2/0/2 et 2/0/4)
```
] int gi 2/0/2
2/0/2] port link-aggregation group 1
2/0/2] int gi 2/0/4
2/0/4] port link-aggregation group 1
```

Configuration de l'interface aggrégée (les vlans sont évidemment aussi à adapter, dans notre cas, se référer à [la documentation physique](../../2 - Infrastructure/physique.md)
```
] int bridge-aggregation 1
brige-aggregation1] port link-type trunk
brige-aggregation1] port trunk permit vlan 
```

