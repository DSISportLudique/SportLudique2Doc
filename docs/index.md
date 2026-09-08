# Sport Ludique Documentation

## Présentation du projet

Cette documentation permet de décrire l'architecture de Sport Ludique. \
Les serveurs tournent majoritairement sous Linux. \
```
PROXMOX (Virtualisation)
|--Web Server/Auth Portal
|--DNS Server
|--DHCP Server
|--Web Server/Web App
|--Zabbix Server/Monitoring
|--Ticketing Solution
|--Backup Server/Proxmox Backup Server
|--Switch config
|--Router config
|--FW config
```

```
Adresses Sites
|--"Faire tous les sites"
|--Blois |172.28.32.0-172.28.63.255|
