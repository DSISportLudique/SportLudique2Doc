# AD 

L'AD sera hébergé sur un serveur Windows Core  et servira aussi de DC  

## Réseau
L'AD n'a qu'une interface réseau, dans le réseau serveur, sont IP est dans notre cas: `172.28.34.50`  
Il sert de DNS, il est son DNS
Potentiellement, renommer l'interface, la notre s'appellera VLAN-SERVERS

Dans SConfig:  
```
8 (parametres réseau)
X (id de l'interface)
```
Suivre les étapes à l'écran pour configurer suivant la conf énoncée plus haut  

Pour éviter certaines erreurs (notamment d'authentification Kerberos plus tard), il faut désactiver l'ip v6:  
`Disable-NetAdapterBinding -Name "VLAN-SERVERS" -ComponentID "ms_tcpip6" -Confirm:$false` 


## DC
Pour servir de DC il faut installer la feature de domaines:
`Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools`  

Il faut ensuite créer une forêt, notre domaine est `blois.sportludique.fr`:  
`Install-ADDSForest -DomainName "blois.sportludique.fr"`

Son nom doit aussi être BLO-AD

