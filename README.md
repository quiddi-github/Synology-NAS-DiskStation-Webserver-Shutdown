"# Synology-NAS-DiskStation-Webserver-Shutdown" 
<br>
Stand 23.2.2025<br>
<br>
Das Skript basiert auf https://github.com/developsessions/synology-nas-webserver-shutdown?tab=readme-ov-file
Dist ist so nicht mehr nutzbar, da Synology init nicht mehr benutzt sondern die Systemd. Deshalb habe ich es umgeschrieben.<br>
<br>
Hierzu bitte wie wolgt vorgehen:<br>
1. Python 3 über den Paketmanager auf der Weboberfläche der Synology NAS installieren. Mittlerweile ist es Version 3.9.<br>
<br>
2. Bitte mittels SSH nachschauen, ob Python auch wirklich in dem Ordner<br>
/usr/local/bin/python3.9
liegt. Wenn neiu, dies bitte in webserver_shutdown.service anpassen.<br>
<br>
3. Per SSH nachschauen, ob es den Service pkgctl-Python3.9.service gibt. Hierzu<br>
`systemctl list-unit-files`<br>
ausführen und schauen, ob pkgctl-Python3.9 hier irgendwo steht. Ansonsten sollte es in der webserver_shutdown.service angepasst werden.<br>
<br>
4. Die Dateien
Mittels SCP mit<br>
`scp webserver_shutdown* youruser@192.168.1.2:/tmp`<br>
übertragen.<br>
<br>
5. Das Python Script in /usr verschieben mit <br>
`sudo mv /tmp/webserver_shutdown.py /usr/`<br>
und den service mit 
`sudo mv /tmp/webserver_shutdown.service /etc/systemd/system/`<br>
verschieben.
<br>
6. Danach alles testen mit<br>
`sudo systemctl daemon-reload`<br>
`sudo systemctl enable webserver_shutdown`<br>
`sudo systemctl start webserver_shutdown`<br>
Das sollte ohen Fehler durchlaufen. Andernfalls bitte debiggen.<br>
7. Danach mit<br>
`http://192.168.YourNASIP.1:8080`<br>
im Browser deiner Wahl schauen, ob der WEbserver aufgeht.<br>
<br>
8. Schauen, ob es nach einem Reboot immer noch `tes` läuft.
```sh
#!/bin/bash
echo "Hallo Welt!"
ls -la
```