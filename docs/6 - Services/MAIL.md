# Serveur de Mail
## Dovecot + Postfix

### Paquets nécessaires

`sudo apt install -y dovecot-core dovecot-imapd dovecot-lmtpd dovecot-mysql postfix mariadb-server`

### Début installation

#### Base de données
```
[None] CREATE DATABASE mailserver;
[None] USE mailserver;
[mailserver] CREATE TABLE users (
   > username varchar(100) NOT NULL;
   > domain varchar(100) NOT NULL );
[mailserver] CREATE USER 'mailuser'@'localhost' IDENTIFIED BY 'pword';
[mailserver] GRANT SELECT TO 'mailuser'@'localhost' ON mailserver.\* ;
[mailserver]exit
```

#### Setup Dovecot

##### Explications Dovecot

Dovecot gère les boites aux lettres locales grâce à imap et lmtp

##### /etc/dovecot/dovecot.conf

```
dovecot_config_version = 2.4.0
dovecot_storage_version = 2.4.0

protocols {
  imap = yes
  lmtp = yes
}

!include_try /usr/share/dovecot/protocols.d/*.protocol

listen = *, ::

!include conf.d/*.conf
!include_try local.conf
```

##### /etc/dovecot/conf.d/10-auth.conf

```
auth_allow_cleartext = yes

!include auth-sql.conf.ext
```
/!\ bien faire attention que ça soit le bon include

##### /etc/dovecot/conf.d/auth-sql.conf.ext

```
mysql /var/run/mysqld/mysqld.sock {
  ssl=no
  user=mailuser
  password=password
  dbname = mailserver
}

passdb sql {
  sql_driver = mysql
  default_password_scheme = SHA512-CRYPT
  query = SELECT username, domain, password FROM users WHERE username='%{user|username}' AND domain='%{user|domain}';
}
```

/etc/dovecot/conf.d/10-mail.conf

```
mail_home = /var/mail/%{user|domain}/%{user|username}
mail_driver = maildir
mail_path = ~/Maildir
mail_uid=vmail
mail_gid=vmail

namespace inbox {
  inbox = yes
}

protocol !indexer-worker {
}
```

##### /etc/dovecot/conf.d/10-master.conf

```
service imap-login {
  inet_listener imap {
    port = 143
  }
  inet_listener imaps {
    #port = 993
    #ssl = yes
  }

}

service pop3-login {
  inet_listener pop3 {
    #port = 110
  }
  inet_listener pop3s {
    #port = 995
    #ssl = yes
  }
}

service submission-login {
  inet_listener submission {
    #port = 587
  }
  inet_listener submissions {
    #port = 465
  }
}

service lmtp {
  unix_listener /var/spool/postfix/private/dovecot-lmtp {
    mode = 0666
    user = postfix
    group = postfix
  }

}

service imap {
  # Most of the memory goes to mmap()ing files. You may need to increase this
  # limit if you have huge mailboxes.
  #vsz_limit = 256M # default

  # Max. number of IMAP processes (connections)
  #process_limit = 1024
}

service pop3 {
  # Max. number of POP3 processes (connections)
  #process_limit = 1024
}

service submission {
  # Max. number of SMTP Submission processes (connections)
  #process_limit = 1024
}

service auth {
  unix_listener auth-userdb {
    #mode = 0666
    #user =
    #group =
  }

   #Postfix smtp-auth
  unix_listener /var/spool/postfix/private/auth {
    mode = 0666
    user = postfix
    group = postfix
  }

  # Auth process is run as this user.
  #user = $SET:default_internal_user
}

service auth-worker {
  # Auth worker process is run as root by default, so that it can access
  # /etc/shadow. If this isn't necessary, the user should be changed to
  # $SET:default_internal_user.
  #user = root
}

service dict {
  # If dict proxy is used, mail processes should have access to its socket.
  # For example: mode=0660, group=vmail and global mail_access_groups=vmail
  unix_listener dict {
    #mode = 0600
    #user =
    #group =
  }
}
```

##### /etc/dovecot/conf.d/20-lmtp.conf

```
protocol lmtp {
  auth_username_format = %{user | lower}
}
```

##### /etc/dovecot/conf.d/10-ssl.conf

```
ssl = yes
ssl_server_cert_file = /etc/ssl/fullchain.pem
ssl_server_key_file = /etc/ssl/mail.key
ssl_server_ca_file = /etc/ssl/ca-blois.crt
```

#### Setup Postfix

##### Explications

Postfix s'occupe de l'envoi des mails avec smtp

##### /etc/postfix/main.cf

```
compatibility_level = 3.6

myhostname = mail.blois.sportludique.fr
mydomain = blois.sportludique.fr

recipient_delimiter = +

inet_interfaces = all

myorigin = /etc/mailname
smtpd_banner = $myhostname ESMTP $mail_name (Debian)

mynetworks = 127.0.0.0/8 10.5.150.0/24 172.28.32.0/24
mydestination = $myhostname, BLO-MAIL, localhost.localdomain, localhost
relayhost = 

virtual_transport = lmtp:unix:private/dovecot-lmtp
virtual_mailbox_domains = blois.sportludique.fr
virtual_mailbox_maps = proxy:mysql:/etc/postfix/sql/mailboxes.cf

smtpd_tls_security_level = encrypt

smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_auth_enable = yes

smtpd_tls_cert_file = /etc/ssl/fullchain.pem
smtpd_tls_key_file = /etc/ssl/mail.key
smtpd_tls_CAfile = /etc/ssl/ca-blois.crt

smtpd_tls_mandatory_protocols = >=TLSv1.2
smtpd_tls_protocols = >=TLSv1.2

smtpd_tls_loglevel = 2

tls_medium_cipherlist = ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA
tls_preempt_cipherlist = yes

maillog_file = /var/log/maillog
```

##### /etc/postfix/master.cf

```
submission inet n   -   y   -   -   smtpd
  -o syslog_name=postfix/submission
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_reject_unlisted_recipient=no
  -o smtpd_forbid_unauth_pipelining=no

smtp inet   n   -   -   -   -   smtpd
  -o syslog_name=postfix/smtp
  -o smtpd_sasl_auth_enable=yes
```
/!\ Laisser les autres valeurs par défaut

##### /etc/postfix/sql/domains.cf

```
user = mailuser
password = password
hosts = localhost
dbname = mailserver
query = SELECT DISTINCT domain FROM users WHERE domain='%s'
```

##### /etc/postfix/sql/

```
user = mailuser
password = password
hosts = localhost
dbname = mailserver
query = SELECT 1 FROM users WHERE username='%u' AND domain='%d'
```
