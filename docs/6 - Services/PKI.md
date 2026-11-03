# PKI

## Configuration

### Socle

- Retirer l'interface réseau LAN et garder uniquement l'interface de Management  

créer dossier "autorité" avec tous les sous-dossiers utiles  
```
mkdir -p autorite/cles autorite/certificats autorite/demandes autorite/newcerts autorite/crl \
serveurs/demandes serveurs/certificats
```

- Créer la clé privée du CA:  
```
openssl genpkey -algorithm RSA -aes-256-cbc \
 -pkeyopt rsa_keygen_bits:4096 \
 -out autorite/cles/ca-blois.key
```

- Créer le certificat autosigné du CA:  
```
openssl req -new -x509 -days 365 \
  -key autorite/cles/ca-blois.key \
  -out autorite/certificats/ca-blois.crt
```

- Récupérer la demande de certificat du serveur web au moyen de scp  

- Signer la CSR du serveur Web:  
```
openssl x509 -req -days 365 \
  -in serveur/demandes/web.csr \
  -CA autorite/certificats/ca-<ville>.crt \
  -CAkey autorite/cles/ca-.key \
  -CAserial autorite/ca.srl \
  -CAcreateserial \
  -out serveur/certificats/web.crt \
  -copy_extensions copy
```

- Renvoyer la clé sur le serveur Web au moyen de scp  

### Avancé

- Créer une demande de Certification de l'autorité de certification interne:  
```
openssl req -new -sha256 \
  -key autorite/cles/ca-blois.key \
  -out autorite/demandes/ca-groupe.csr \
  -subj "/C=FR/O=SportLudique/CN=CA Blois SportLudique"
```

- Envoyer la demande à l'autorité de certification  

- Vérifier les empreintes/hash  

- Puis:
```
mkdir -p autorite/newcerts autorite/crl
touch autorite/index.txt
openssl rand -hex 16 > autorite/serial
printf '1000\n' > autorite/crlnumber
```

- Créer 'autorite/openssl.cnf':  
`vim autorite/openssl.cnf`
Contenu:
```
[ ca ]
default_ca = CA_default

[ CA_default ]
dir = ./autorite
database = $dir/index.txt
new_certs_dir = $dir/newcerts
serial = $dir/serial
crlnumber = $dir/crlnumber
certificate = $dir/certificats/ca-blois.crt
private_key = $dir/cles/ca-blois.key
default_md = sha256
default_days = 90
default_crl_days = 7
policy = policy_server
copy_extensions = none
unique_subject = no

[ policy_server ]
countryName = optional
organizationName = optional
commonName = supplied
```

- Emettre un nouveau certificat serveur:  
`vim serveurs/demandes/web.ext`
Contenu:
```
basicConstraints = critical, CA:FALSE
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
subjectAltName = DNS:www.chartres.sportludique.fr,DNS:chartres.sportludique.fr
```

- Signez le certificat:  
```
openssl ca -config autorite/openssl.cnf \
  -in serveur/demandes/web.csr \
  -extfile serveur/demandes/web.ext \
  -out serveur/certificats/web-intermediaire.crt \
  -notext -days 200
```

- Vérifier la chaîne complète:  
```
openssl verify -show_chain \
  -verify_hostname "www.ville.sportludique.fr" \
  -CAfile autorite/certificats/sportludique-root.crt.pem \
  -untrusted autorite/certificats/ca-intermediaire.crt \
  serveur/certificats/web-intermediaire.crt
```

- Créer le fichier de la chaîne de confiance:
```
cat serveur/certificats/web-intermediaire.crt \
    autorite/certificats/ca-intermediaire.crt \
    > serveur/certificats/fullchain.pem
```
- Envoyer la chaîne de confiance au serveur Web.
