# IMAP

## Setup Certificats

**Depuis le serveur CA*

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

- Créer le fichier de la chaîne de confiance:
```
cat serveur/certificats/web-intermediaire.crt \
    autorite/certificats/ca-intermediaire.crt \
    > serveur/certificats/fullchain.pem
```
