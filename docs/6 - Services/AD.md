# AD 

L'AD sera hébergé sur un serveur Windows Core  et servira aussi de DC  

## DC
Pour servir de DC il faut installer la feature de domaines:
`Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools`  

Il faut ensuite créer une forêt, notre domaine est `blois.sportludique.fr`:  
`Install-ADDSForest -DomainName "blois.sportludique.fr"`

