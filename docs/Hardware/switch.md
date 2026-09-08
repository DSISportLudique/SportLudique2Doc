# Switch

## Switches utilisés
HP A5500 Stack

## Reset
Dans un premier temps, il faut reset le switch (s'il n'est pas neuf)

```
> reset saved-configuration 
> reset saved-configuration main
> reset saved-configuration backup 
```
### Recherche des fichiers restants
```
> dir /all 
```
### Supprimer chaque .cfg (si le nom est entouré de [nom], faire `undelete nom` puis le supprimer comme suit) avec cette commande:
```
delete /unreserved fichier
```

### S'assurer d'etre le master 1 du stack (car stand alone)
```
> display irf
```
Il sera affiché: *+X
```
> system-view
] irf member X renumber 1
```

## Config

