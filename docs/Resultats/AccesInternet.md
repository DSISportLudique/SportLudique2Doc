# Acces à Internet

## Traceroute
### Req
- Acces à un vlan inférieur (client dans le test)  
- NAT on sur le routeur 

### Preuve de résultat:
Le 1 apparait en * * * car caché par défaut, pour l'afficher, se référer à la config générale
``` 
~ $ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  * * *
 2  192.168.69.254 (192.168.69.254)  2.630 ms  2.680 ms  2.742 ms
 3  softbank221087141002.bbtec.net (221.87.141.2)  1.463 ms  1.425 ms  1.434 ms
 4  121.183.90.206 (121.183.90.206)  1.518 ms  1.583 ms  1.543 ms
 5  172.16.200.254 (172.16.200.254)  3.648 ms  3.664 ms  3.931 ms
 6  172.16.60.1 (172.16.60.1)  2.527 ms  1.430 ms  1.522 ms
 7  192.168.10.2 (192.168.10.2)  4.354 ms  4.679 ms  5.162 ms
 8  10.99.6.142 (10.99.6.142)  2.324 ms  2.113 ms  2.086 ms
 9  * * *
10  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  3.321 ms  3.260 ms  3.319 ms
11  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  3.602 ms  3.609 ms  3.091 ms
12  te0-0-0-8-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.84)  6.293 ms te0-2-0-30-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.90)  6.935 ms te0-2-0-31-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.92)  6.703 ms
13  et-5-5-1-ren-nr-paris2-rtr-131.noc.renater.fr (193.51.180.42)  5.548 ms  5.630 ms  5.619 ms
14  equinix-paris.cloudflare.com (195.42.144.143)  13.594 ms  12.955 ms  12.959 ms
15  141.101.67.142 (141.101.67.142)  6.028 ms  5.834 ms 141.101.67.143 (141.101.67.143)  5.685 ms
16  141.101.67.167 (141.101.67.167)  7.044 ms 141.101.67.165 (141.101.67.165)  12.494 ms 141.101.67.153 (141.101.67.153)  5.903 ms
17  one.one.one.one (1.1.1.1)  6.394 ms  6.299 ms  6.259 ms
``` 

