# Résolveur DNS

## unbound configuration

**En premier temps installer le package unbound**
`sudo apt install -y unbound`

fichier de configuration: /etc/unbound/unbound.conf

```
server:
    do-ip4: yes
    do-ip6: no

    do-udp: yes
    do-tcp: yes

    port: 53

    interface: 127.0.0.1
    interface: 172.28.34.53
    interface: 10.5.150.153

    access-control: 127.0.0.1 allow
    access-control: 172.28.32.0/19 allow
    access-control: 10.5.150.0/24 allow

    outgoing-interface: 172.28.34.53

stub-zone:
    name: blois.sportludique.fr
        stub-addr: 192.168.32.53

forward-zone:
    name: sportludique.fr
        forward-addr: 121.183.90.205

include-toplevel: "/etc/unbound/unbound.conf.d/*.conf"
```

Pour tester la configuration: `unbound-checkconf`

## Preuves


## Local
```
nslookup cheese.com
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   cheese.com
Address: 195.149.84.43
Name:   cheese.com
Address: 2001:67c:38c::2b
```

### VLAN Serveurs
```
nslookup cheese.com
Server:         172.28.34.53
Address:        172.28.34.53#53

Non-authoritative answer:
Name:   cheese.com
Address: 195.149.84.43
Name:   cheese.com
Address: 2001:67c:38c::2b
```
