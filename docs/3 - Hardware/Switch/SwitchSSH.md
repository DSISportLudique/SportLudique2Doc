# SSH Setup Switch HP A5500

## Notations
Les notations sont définies dans [Notations](../../1 - Guidelines/Notations.md)  

## Notations
Dans les parties commandes:
Mode User: `>`
Mode system-view: `]`
Mode Interface: `X/X/X]` ou `Vlan X]`
Mode Création User: `User]`

## Création du VLAN de Management (VLAN 150)

*VLAN défini dans le cahier des charges*
```
>system-view
]vlan 150
Vlan 150]quit
```

## Insertion du port de choisit dans le VLAN de Management

*Adapter vos ports à vos besoins*
```
>system-view
]interface GigabitEthernet1/0/32
1/0/32]port access vlan 150
1/0/32]quit
]ssh server enable
]interface GigabitEthernet1/0/31
1/0/31]port access vlan 150
1/0/31]quit
```

## Adresse IP du VLAN

*Adresse IP cachée*
```
>system-view
]interface Vlan-Interface 150
Vlan 150]ip address X.X.X.X 255.255.255.0
Vlan 150] quit
```

## Création des utilisateurs administrateurs du switch

```
>system-view
]local-user M3Enzo
User]authorization-attribute level 3
User]service-type ssh
User]password cipher <password>
User]quit

User]local-user M3lucas
User]authorization-attribute level 3
User]service-type ssh
User]password cipher <password>
User]quit
```

## Finition

```
]user-interface vty 0 15
]authentication-mode scheme
]return
>save safely force
```

## SSH depuis le client
Le switch est vieux, il ne supporte pas les protocoles récents, il faut donc ajouter par ex, ces lignes dans `~/.ssh/config`:  
```
Host sw
        HostName 10.5.150.1
        User USER
        Port 22
        KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
        PubkeyAcceptedAlgorithms +ssh-rsa
        HostkeyAlgorithms +ssh-rsa
        Ciphers aes128-cbc,3des-cbc
```
