---
title: MariaDB Upgrade auf Debian/Ubuntu Systemen
description: 
published: true
date: 2026-04-06T19:15:10.608Z
tags: 
editor: markdown
dateCreated: 2026-04-06T18:50:45.763Z
---

# MariaDB Upgrade auf Debian/Ubuntu Systemen

Bei einem MariaDB Upgrade von, z.b. Version 10 auf Version 11, bleiben alle aktuellen Datenbanken bestehen. Es gibt also keinen Datenverlust, alles läuft nach dem Upgrade genauso wie vorher.\
Ich empfehle dennoch ein vollständiges Backup zu erstellen! Ich übernehme keine Haftung falls etwas schief läuft.

Zunächst einmal prüfen wir welche MariaDB Version wir aktuell installiert haben. Dazu gehen wir natürlich in unsere SSH Konsole und geben folgendes ein:

```bash
mysql
status
exit
```

Hier sehen wir nun bei `Server Version` welche MariaDB Version wir aktuell installiert haben. In diesem Fall gehen wir von Version 10.X aus also müsste hier sowas wie: `Server Version: 10.5.21-MariaDB-XXXXX` stehen.

Um das Upgrade durchführen zu können müssen wir den MariaDB Server natürlich erst einmal stoppen und danach sicherheitshalber Backups erstellen sollte doch etwas schief gehen.

```bash
service mysql stop
cp -R /etc/mysql/ /etc/mysql-backup
cp -R /var/lib/mysql /var/lib/mysql-backup
```

Nun haben wir den MariaDB Server gestoppt und Backups der Dateien erstellt.\
Als nächstes müssen wir die neue Repository zu sources.list hinzufügen. Das geht ganz einfach mit folgendem Befehl:

```bash
curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | sudo bash
```

Als nächstes müssen wir den alten MariaDB Server löschen damit wir die neue Version im Anschluss herunterladen können. Das ganze können wir wie folgt tun:

```bash
apt remove mariadb-*
apt install mariadb-server
```

Im Anschluss sollten wir noch prüfen ob der MariaDB Server gestartet wurde und wenn nicht starten wir diesen einfach manuell.

```bash
mysql
```

Falls wir hier nun einen Fehler erhalten starten wir den MariaDB Server einfach manuell und überprüfen im selben Zuge auch ob die neue Version installiert wurde.

```bash
service mysql start
status
exit
```

In seltenen Fällen gibt es nach der Installation ein paar kleinere Probleme wenn man sich nun, mit z.b. phpmyadmin, versucht anzumelden. Dieser Fehler könnte z.b. wie folgt aussehen:

```
error #1231 – Variable ‘lc_messages’ can’t be set to the value of de_DE
```

In diesem Fall könnte es sein, dass noch ein veralteter Pfad in der Konfiguration hinterlegt ist. Dazu gehen wir einfach in die `/etc/mysql/mariadb.conf.d/50-server.cnf` und ändern den Pfad. In meinem Fall sieht das dann so aus:

```
lc-messages-dir = /usr/share/mariadb/german
lc-messages     = de_DE
```

Nun starten wir einfach den MySQL Server neu und es sollte alles wieder einwandfrei funktionieren.\
Wenn wir getestet haben ob wirklich alles wieder korrekt funktioniert und alle vorher erstellten Datenbanken noch funktionieren können wir die vorhin erstellten Backups einfach wieder löschen.

```bash
rm -r /etc/mysql-backup
rm -r /var/lib/mysql-backup
```

**FERTIG**
