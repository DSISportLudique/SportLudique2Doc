# Stack Switch

## Notations
Mode normal:
`>` \
Mode system:
`]`

## Conf voulue
Avec les 2 switch, on veut le master (switch 1, piorité la plus élevée (32)) et le slave (switch 2, priorité 31) \
Si nécéssaire, reset les switch et configurer le master

## Conf actuelle
```
> sys
] display irf
```
Le \*+X indique le nombre actuel

## Master
<span style="color:red"> */!\ LES PORTS D'INTERFACES SONT POTENTIELLEMENT À ADAPTER* </span> 
```
> sys
] irf member X renumber 1
] quit
> save
> reboot
```

```
> sys
] interface Ten-GigabitEthernet 1/0/1
1/0/1] shutdown
1/0/1] quit
] irf-port 1/1
1/1] port group interface Ten-GigabitEthernet 1/0/1
1/1] quit
] interface Ten-GigabitEthernet 1/0/1
1/0/1] undo shutdown
1/0/1] quit
] quit
> save
> reboot
```

## Slave
<span style="color:red"> */!\ LES PORTS D'INTERFACES SONT POTENTIELLEMENT À ADAPTER* </span> 
```
> sys
] irf member X renumber 2
] quit
> save
> reboot
```

```
> sys
] interface Ten-GigabitEthernet 2/0/1
1/0/1] shutdown
1/0/1] quit
] irf-port 2/1
1/1] port group interface Ten-GigabitEthernet 2/0/1
1/1] quit
] interface Ten-GigabitEthernet 2/0/1
1/0/1] undo shutdown
1/0/1] quit
] quit
> save
> reboot
```

