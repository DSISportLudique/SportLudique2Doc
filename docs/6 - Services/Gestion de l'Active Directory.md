# Gestion de l'Active Directory

## Installation du rôle
Tout d'abord il faut ajouter les outils nécéssaire au Serveur Windows avec GUI
Aller dans "Gérer" -> "Ajouter des rôles et fonctionnalités"

Sélectionner le Serveur: BLO-WIN-GUI.blo.blois.sportludique.fr
Aller dans la partie "Fonctionnalités"
Dérouler le sous-menu "Outils d'administration de serveur distant" puis "Outils d'administration de rôles"
"Outils AD DS et AD LDS" et cocher "Outils AD DS" ça vous demandera d'ajouter d'autres modules nécéssaires, appuyer sur "OK" et ils seront ajoutés automatiquement.

Finir la sélection et installer.

## Gestion des OU

Dans le menu "Outils" aller sur "Utilisateurs et ordinateurs Active Directory"
Ajouter un OU dans le domaine "blo.blois.sportludique.fr", cliquer sur le domaine puis aller dans "Actions" -> "Nouveau"
Créer une nouvelle Unité Organisationnelle, nommée "BLO-Admins"
Créer deux utilisateurs nominatifs dans l'OU
les ajouter dans les groupes "Administrateurs" et "Admins du domaine"
