# Serveur d'autoritée

## Outils
Le serveur est sous Debian, il utilisera bind9 conformément à [la doc](https://lmeryfulbert.github.io/SportLudique2026-2027/cours/05-Services/dns/03-dns-avance/)  

Pour les tests, les commandes suivantes peuvent servir:  
- `named-checkconf`: check la config bind  
- `named-checkzone file`: check la config de la zone décrite dans file  

## Configuration
Le serveur doit avoir autorité sur la zone `blois.sportludique.fr`  
Le DNS doit faire un split, il doit donc donner des ip différentes aux requêtes provenants d'internet et à celles du LAN
On créer donc une zone `inside` et une `outside`

### named.conf
Configuration globale et du split
Dans `/etc/bind/named.conf`:  
```
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";

acl "local-networks" {
        127.0.0.1;
        172.28.32.0/19;
        192.168.68.0/24;
};

view "inside" {
    match-clients {
            local-networks;
    };

    zone "blois.sportludique.fr" {
            type master;
            file "/etc/bind/db.blois.sportludique.fr.in";
    };

    include "/etc/bind/named.conf.root-hints";
}; 

view "outside" {
    match-clients {
            any;
    };

    zone "blois.sportludique.fr" {
            type master;
            file "/etc/bind/db.blois.sportludique.fr.out";
    };

    include "/etc/bind/named.conf.root-hints";
};
```

### named.conf.options
Options générales + log  

Dans `/etc/bind/named.conf.options`:  
```
options {
        directory "/var/cache/bind";

        listen-on { any; };
        listen-on-v6 { none; };

        dnssec-validation no;
        recursion yes;
};

logging {
     channel default_log {
          file "/var/log/named/default" versions 3 size 20m;
          print-time yes;
          print-category yes;
          print-severity yes;
          severity info;
     };

     category default  { default_log; };
     category queries  { default_log; };
     category network  { default_log; };
     category config   { default_log; };
     category cname    { default_log; };
     category security { default_log; };
     category client   { default_log; };
     category resolver { default_log; };
};

```

### Zone interne
Dans `/etc/bind/db.blois.sportludique.fr.in`:  
```
;/etc/bind/db.blois.sportludique.fr.in

$ORIGIN blois.sportludique.fr.
$TTL 2h

@               SOA     ns1 hostmaster (
                                1210202605 ; Serial (DDMMYYYYnn)
                                8h         ; Refresh
                                30m        ; Retry
                                1w         ; Expire
                                1h )       ; Negative Cache TTL
@               IN      NS      ns1

;blois.sportludique.fr. IN      A       192.168.32.53
blo             IN      A       172.28.34.50

ns1             IN      A       192.168.32.53
```

### Zone externe
Dans `/etc/bind/db.blois.sportludique.fr.out`:  
```
;/etc/bind/db.blois.sportludique.fr.out

$ORIGIN blois.sportludique.fr.
$TTL 2h

@               SOA     ns1 hostmaster (
                                1210202613 ; Serial (DDMMYYYYnn)
                                8h         ; Refresh
                                30m        ; Retry
                                1w         ; Expire
                                1h )       ; Negative Cache TTL
@       IN      NS      ns1

ns1     IN      A       183.44.41.2
ns2     IN      A       221.87.141.1
```

