# Synology-NAS-DiskStation-Webserver-Shutdown

Stand 23.2.2025

Das Skript basiert auf [https://github.com/developsessions/synology-nas-webserver-shutdown](https://github.com/developsessions/synology-nas-webserver-shutdown?tab=readme-ov-file).  
Die Distribution ist so nicht mehr nutzbar, da Synology `init` nicht mehr verwendet, sondern auf `systemd` umgestiegen ist. Deshalb habe ich es umgeschrieben.

Hierzu bitte wie folgt vorgehen:

1. Python 3 über den Paketmanager auf der Weboberfläche der Synology NAS installieren. Mittlerweile ist es Version 3.9.

2. Bitte mittels SSH nachschauen, ob Python auch wirklich im Ordner `/usr/local/bin/python3.9` liegt. Wenn nicht, dies bitte in `webserver_shutdown.service` anpassen.

3. Per SSH nachschauen, ob es den Service `pkgctl-Python3.9.service` gibt. Hierzu 
`systemctl list-unit-files`
ausführen und schauen, ob `pkgctl-Python3.9` hier irgendwo steht. Ansonsten sollte es in der `webserver_shutdown.service` angepasst werden.

4. Die Dateien mittels SCP mit 
`scp webserver_shutdown* youruser@192.168.1.2:/tmp`
übertragen.

5. Das Python-Skript in `/usr` verschieben mit 
`sudo mv /tmp/webserver_shutdown.py /usr/`
und den Service mit
`sudo mv /tmp/webserver_shutdown.service /etc/systemd/system/`
verschieben.

6. Danach alles testen mit:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable webserver_shutdown
   sudo systemctl start webserver_shutdown
   ```
Das sollte ohen Fehler durchlaufen. Andernfalls bitte debiggen.

7. Danach mit
`http://192.168.YourNASIP.1:8080`
im Browser deiner Wahl schauen, ob der WEbserver aufgeht.

8. Schauen, ob es nach einem Reboot immer noch läuft.

Wen es stört, dass bei dem Befehl`sudo` immer nochmal das Passwort abgefragt wird, kann das mit
`sudo vim /etc/sudoers` und dem Eintrag
`benutzername ALL=(ALL) NOPASSWD: ALL` ändern.