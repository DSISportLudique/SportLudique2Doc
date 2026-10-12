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

forward-zone:
    name: blois.sportludique.fr
        forward-addr: 192.168.32.53

include-toplevel: "/etc/unbound/unbound.conf.d/*.conf"
```

## Preuves


## Local
```
nslookup youtube.com
Server:         127.0.0.1
Address:        127.0.0.1#53

Non-authoritative answer:
Name:   youtube.com
Address: 172.217.22.206
Name:   youtube.com
Address: 2a00:1450:4007:81d::200e
```

## VLAN Management
```
nslookup youtube.com
Server:         10.5.150.153
Address:        10.5.150.153#53

Non-authoritative answer:
Name:   youtube.com
Address: 172.217.22.206
Name:   youtube.com
Address: 2a00:1450:4007:81d::200e
```

### VLAN Serveurs
```
nslookup youtube.com
Server:         172.28.34.53
Address:        172.28.34.53#53

Non-authoritative answer:
Name:   youtube.com
Address: 172.217.22.206
Name:   youtube.com
Address: 2a00:1450:4007:81d::200e
```
