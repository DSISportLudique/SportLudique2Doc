# DNS

## Autoritée

### Exemple faux
Une requête qui ne fonctionne pas ressemblerais à ça:
```
$ dig @172.28.34.53 chartres.sportludique.fr

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> @172.28.34.53 chartres.sportludique.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: SERVFAIL, id: 14874
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;chartres.sportludique.fr.      IN      A

;; Query time: 1140 msec
;; SERVER: 172.28.34.53#53(172.28.34.53) (UDP)
;; WHEN: Mon Oct 12 15:45:14 UTC 2026
;; MSG SIZE  rcvd: 53


```

### Interne
```
$ dig @172.28.34.53 blois.sportludique.fr

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> @172.28.34.53 blois.sportludique.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 35588
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;blois.sportludique.fr.         IN      A

;; AUTHORITY SECTION:
blois.sportludique.fr.  2765    IN      SOA     ns1.blois.sportludique.fr. hostmaster.blois.sportludique.fr. 1210202605 28800 1800 604800 3600

;; Query time: 0 msec
;; SERVER: 172.28.34.53#53(172.28.34.53) (UDP)
;; WHEN: Mon Oct 12 15:44:27 UTC 2026
;; MSG SIZE  rcvd: 101

```

### Externe

#### Sur le firewall

```
$ dig @192.168.69.1 blois.sportludique.fr

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> @192.168.69.1 blois.sportludique.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 50984
;; flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 1b3ac76c33fc4346010000006acd01a556b00af8b9cacad6 (good)
;; QUESTION SECTION:
;blois.sportludique.fr.         IN      A

;; AUTHORITY SECTION:
blois.sportludique.fr.  3600    IN      SOA     ns1.blois.sportludique.fr. hostmaster.blois.sportludique.fr. 1210202613 28800 1800 604800 3600

;; Query time: 0 msec
;; SERVER: 192.168.69.1#53(192.168.69.1) (UDP)
;; WHEN: Mon Oct 12 15:49:57 UTC 2026
;; MSG SIZE  rcvd: 129
```

#### Sur le DNS, depuis l'internet simulé

```
$ dig @121.183.90.205 blois.sportludique.fr

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> @121.183.90.205 blois.sportludique.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36330
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;blois.sportludique.fr.		IN	A

;; AUTHORITY SECTION:
blois.sportludique.fr.	3587	IN	SOA	ns1.blois.sportludique.fr. hostmaster.blois.sportludique.fr. 1210202613 28800 1800 604800 3600

;; Query time: 4 msec
;; SERVER: 121.183.90.205#53(121.183.90.205) (UDP)
;; WHEN: Tue Oct 13 07:47:31 CEST 2026
;; MSG SIZE  rcvd: 101
```

