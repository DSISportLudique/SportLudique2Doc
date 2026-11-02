# PKI

## Configuration

- Retirer l'interface réseau LAN et garder uniquement l'interface de Management  

créer dossier "autorité" avec "cles" et "certificats" en sous-dossiers
`mkdir -p autorite/cles`
`mkdir -p autorite/certificats`

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
