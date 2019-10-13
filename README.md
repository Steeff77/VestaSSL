VestaCP System SSL
First:(v-add-letsencrypt-domain) the VestaCP SSL.
Than
```nano /etc/cron.daily/ssl_renew```
Please change the [USER] and [DOMAIN] part with your system domain.

```text
#!/bin/bash

cert_src="/home/[USER]/conf/web/ssl.[DOMAIN].pem"
key_src="/home/[USER]/conf/web/ssl.[DOMAIN].key"
cert_dst="/usr/local/vesta/ssl/certificate.crt"
key_dst="/usr/local/vesta/ssl/certificate.key"

if ! cmp -s $cert_dst $cert_src
then
        # Copy Certificate
        cp $cert_src $cert_dst

        # Copy Keyfile
        cp $key_src $key_dst

        # Change Permission
        chown root:mail $cert_dst
        chown root:mail $key_dst

        # Restart Services
        service vesta restart &> /dev/null
        service exim4 restart &> /dev/null
fi
```

```chmod +x /etc/cron.daily/ssl_renew```
