# WEB

Le serveur web utilisera apache  
Il y aura plusieurs VHost:  
- www.blois.sportludique.fr
- salut.blois.sportludique.fr
<!-- plus tard, aussi celui pour les slam -->

## HTTP
### Bases
installation apache: `apt install apache2`  
desactiver un site (ici le site par défaut): `a2dissite 000-default`  
activer un site (ici www): `a2ensite www.blois.sportludique.fr`  
Pour la base: 
```
cp /etc/apache2/sites-available/000-default.conf \
    /etc/apache2/sites-available/{www,salut}.blois.sportludique.fr
```

### Configuration des sites
Avec évidemment le bon url  
```
<VirtualHost *:80>
	ServerName url

	DocumentRoot /var/www/blois/

	ErrorLog /var/log/apache2/blois/error.log
	CustomLog /var/log/apache2/blois/access.log combined
</VirtualHost>

```

