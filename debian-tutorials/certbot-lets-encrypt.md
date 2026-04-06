# Certbot – Let’s Encrypt (kostenlose SSL-Zertifikate)

Es ist eigentlich ziemlich einfach gehalten. Man muss nur Certbot installieren und schon kann man sich SSL Zertifikate kostenlos über Let’s Encrypt holen.

```bash
apt install certbot python-certbot-apache -y
certbot --authenticator webroot --installer apache -w /var/www/html/{verzeichnis} -d deine-domain.de -d www.deine-domain.de
```

Beim ersten Start muss eine Email Adresse angegeben werden und man muss mit „A“ die Lizenzbedingungen akzeptieren.

Die Zertifikate halten nur 90 Tage lang, im Normalfall sollten diese aber automatisch erneuert werden. Falls dies nicht der Fall ist folgendes Ausführen:

```bash
certbot renew
```
