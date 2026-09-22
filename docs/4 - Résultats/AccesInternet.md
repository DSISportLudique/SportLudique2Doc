# Acces à Internet

## Traceroute
### Req
- Acces à un vlan inférieur (client dans le test)  
- NAT on sur le routeur 

### Preuve de résultat:
Le 1 apparait en * * * car caché par défaut, pour l'afficher, se référer à la [config générale](../3 - Hardware/Switch/SwitchHP.md/#nb)
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

## HSRP
Avec la configuration dans [HSRP](../../5 - Hardware/Switch/hsrp.md)

### Routeur 1 OFF
``` 
~ $ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  * * *
 2  192.168.69.252 (192.168.69.252)  3.482 ms  3.721 ms  3.816 ms
 3  183.44.41.1 (183.44.41.1)  1.740 ms  1.696 ms  1.691 ms
 4  121.183.90.206 (121.183.90.206)  1.771 ms  1.727 ms  1.869 ms
 5  172.16.200.254 (172.16.200.254)  1.948 ms  1.904 ms  1.910 ms
 6  172.16.60.1 (172.16.60.1)  3.480 ms  1.925 ms  1.917 ms
 7  192.168.10.2 (192.168.10.2)  4.645 ms  4.787 ms  4.825 ms
 8  10.99.6.142 (10.99.6.142)  2.691 ms  2.683 ms  2.635 ms
 9  * * *
10  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  4.318 ms  5.060 ms  5.098 ms
11  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  5.028 ms  4.066 ms  4.040 ms
12  te0-0-0-19-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.86)  6.680 ms te0-0-0-8-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.84)  6.370 ms te0-2-0-30-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.90)  6.581 ms
13  et-5-5-1-ren-nr-paris2-rtr-131.noc.renater.fr (193.51.180.42)  6.077 ms  6.097 ms  6.146 ms
14  equinix-paris.cloudflare.com (195.42.144.143)  6.382 ms  6.400 ms  6.440 ms
15  141.101.67.143 (141.101.67.143)  6.364 ms 141.101.67.142 (141.101.67.142)  6.161 ms 141.101.67.143 (141.101.67.143)  6.107 ms
16  141.101.67.191 (141.101.67.191)  12.276 ms 141.101.67.187 (141.101.67.187)  13.014 ms 141.101.67.153 (141.101.67.153)  6.179 ms
17  one.one.one.one (1.1.1.1)  7.161 ms  7.027 ms  6.802 ms
``` 

### Routeur 2 OFF
``` 
~ $ traceroute 1.1.1.1
traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets
 1  * * *
 2  192.168.69.253 (192.168.69.253)  2.040 ms  2.086 ms  2.170 ms
 3  221.87.141.2 (221.87.141.2)  1.610 ms  1.562 ms  1.514 ms
 4  121.183.90.206 (121.183.90.206)  1.539 ms  1.426 ms  1.547 ms
 5  172.16.200.254 (172.16.200.254)  9.798 ms  9.748 ms  9.754 ms
 6  172.16.60.1 (172.16.60.1)  2.493 ms  1.038 ms  1.166 ms
 7  192.168.10.2 (192.168.10.2)  4.149 ms  4.268 ms  4.837 ms
 8  10.99.6.142 (10.99.6.142)  2.209 ms  2.158 ms  2.106 ms
 9  * * *
10  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  3.069 ms  3.310 ms  3.210 ms
11  vl1799-be6-ren-nr-orleans-rtr-091.noc.renater.fr (193.51.184.233)  3.342 ms  3.721 ms  3.662 ms
12  te0-2-0-29-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.88)  6.585 ms te0-0-0-8-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.84)  6.072 ms te0-0-0-19-ren-nr-orsay-rtr-091.noc.renater.fr (193.55.204.86)  5.896 ms
13  et-5-5-1-ren-nr-paris2-rtr-131.noc.renater.fr (193.51.180.42)  5.763 ms  5.680 ms  5.578 ms
14  equinix-paris.cloudflare.com (195.42.144.143)  6.957 ms  6.903 ms  6.863 ms
15  141.101.67.142 (141.101.67.142)  5.520 ms 141.101.67.143 (141.101.67.143)  5.561 ms 141.101.67.142 (141.101.67.142)  5.595 ms
16  141.101.67.163 (141.101.67.163)  6.578 ms 141.101.67.179 (141.101.67.179)  6.443 ms 141.101.67.189 (141.101.67.189)  7.124 ms
17  one.one.one.one (1.1.1.1)  6.515 ms  6.324 ms  6.634 ms
``` 
