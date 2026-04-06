# TeamSpeak 3 Server

## Installation

Wir installieren nun einen Teamspeak 3 Server manuell, ohne irgendwelche Installer-Scripte.

```bash
adduser --disabled-login ts3
cd /home/ts3/
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2
tar -xjvf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
cd teamspeak3-server_linux_amd64
chmod +x ts3server_startscript.sh
touch .ts3server_license_accepted
chown -R ts3:ts3 /home/ts3/
su ts3
./ts3server_startscript.sh start
```

Mit dem Befehl `su ts3` loggen wir uns in den `ts3` Benutzer ein, da wir nicht mit dem `root` Benutzer einen Server starten wollen.

Ich empfehle außerdem den Query Port unseres Teamspeak Servers zu ändern. Dazu müssen wir im Startbefehl einfach `query_port=PORT`eingeben. `PORT` ist in diesem Fall der neue Query Port. Das müssen wir bei jedem Start so ausführen.

**Beispiel:** `/home/ts3/teamspeak3-server_linux_amd64/ts3server_startscript.sh start query_port=12345`

**Wichtig:** Admin Server Query Login und den Token kopieren.

_**Kompletter Pfad:**_ _`/home/ts3/teamspeak3-server_linux_amd64/ts3server_startscript.sh {start/stop/restart}`_

[_weiter zu Sinusbot installieren_](sinusbot.md)

***

## Updaten

```bash
cd /home/ts3/
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2
tar -xjvf teamspeak3-server_linux_amd64-3.13.7.tar.bz2
chown -R ts3:ts3 /home/ts3/
cd teamspeak3-server_linux_amd64
su ts3
./ts3server_startscript.sh start
```

Also beim Update einfach den Server mit der neuen Version neu runterladen und entpacken. Die erforderlichen Dateien überschreiben sich dann automatisch.\
Der Rest wie Configs, etc. bleiben erhalten.

***

## Fehler

Sollte der Server mal nicht starten bzw. nach dem Start direkt wieder stoppen, am besten mal in den Logs nach dem Fehler suchen. Alternativ kann man auch `ts3server_minimal_runscript.sh` ausgeführt werden.

Falls hier folgender Fehler zu sehen ist, müssen zwei Befehl ausgeführt werden. Dies kann z.b. der Fall sein, wenn man den Teamspeak Server aus versehen als root Benutzer gestartet hat.

`|` `ERROR | Accounting | could not register the local accounting service: The file already exists`

Nun einmal diesen Befehl als root ausführen: `ls -al /dev/shm/`

Jetzt sollte sowas wie das hier angezeigt werden:\
`-rw-r–r-- 1 root root 128 Dez 5 11:41 7gbhujb54g8z9hu43jre8`

Wenn dies der Fall ist muss diese Datei mit `rm /dev/shm/7gbhujb54g8z9hu43jre8` gelöscht werden.

Jetzt kann man den Teamspeak Server wieder ganz normal mit dem ts3 Benutzer starten.
