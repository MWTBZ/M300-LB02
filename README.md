# M300-LB02
## Einleitung
In der zweiten LB arbeiten wir mit Docker. Ich habe die Vorlage von Herrn Berger genommen, die eine VM mit Docker automatisch installiert.
Ich werde wieder Versuchen einen Webserver und Samba zu installieren. Diesmal natürlich mit Docker
***

## Servicebeschreibung
Wie schon erwähnt, werde ich wieder versuchen **Samba** und zusätzlich einen **Apache** Webserver zu installieren. Ich werde wieder zwei Ordner freigeben und diese unterschiedlich berechtigen. 
Für die Berechtigungen werden zwei Benutzer erstellt, welche je nur auf einen der beiden Ordner Zugriffsrechte haben.    
Der Apache wird einen kleine custom Webseite anzeigen, und im über Samba werden die genannten Ordner ersichtlich sein.
***

## Technische Angaben

### Netzwerkplan
![](/Images/Netzwerkplan.png) 

### Betrieb
Die VM wird mit Vagrant aufgesetzt, damit alle benötigten Ports freigegeben sind. Die Docker mit den benötigten Dateien liegen jeweils im **apache**- und **samba** Ordner.  
Beide Container sind in einem Dockerfile definiert und werden so in Betrieb genommen.   
Damit man die Service benützen kann, müssen ebenfalls im Docker die Ports freigegeben werden.

```
cd /vagrant/apache
docker build -t apache .
docker run --rm -d -p 8080:80 -v `pwd`/web:/var/www/html --name apache apache
```

Um den zweiten Container zu starten müssen wir zurück ins Ursprungsverzeichnis mit `cd ..`.

```
cd /vagrant/samba
docker build -t samba .
docker run -d -p 137:137/udp -p 138:138/udp -p 139:139 -p 445:445 -v /config/:/etc/samba --name samba samba
```
Docker öffnen: `docker run -it apache /bin/bash` und `docker run -it samba /bin/bash`.   
Docker stoppen: `docker stop apache` und `docker stop samba`
Docker löschen: 
```
docker rm `docker ps -a -q`

```


#### smb.conf
Die Konfigurationsdatei von Samba wird vom Client lokal dem Docker übergeben und überschreibt dann die Standardkonfiguration von Samba. Die smb.conf sieht wie folgt aus:

```Properties
# HTML-Share
    [HTMLShare]
        comment = Foldershare for Apache
        path = /var/www/html
        read only = no
        guest ok = no
        writeable = yes
        valid users = samba

# File-Share
    [FileShare]
        comment = Foldershare for Files
        path = /var/FileShare
        read only = no
        guest ok = no
        writeable = yes
        valid users = samba2
```
### Dockerfiles
#### Apache
```Dockerfile

```
#### Samba
```Dockerfile

```
***

## Testing
| Service | Testfall | Beschreibung | Resultat |
|:--:|:--:|:--|:--|
| Webserver | Webseite erreichbar | Browser öffnen<br>**http://10.71.13.20** | Webseite kann aufgerufen werden<br>*Siehe Bild 1* |
| Samba | Samba erreichbar | Windows Explorer öffnen<br> **\\\10.71.13.20** | Freigegebene Ordner<br>werden angezeigt<br>*Siehe Bild 2* |
| Samba | Berechtigung<br>HTMLShare | Windows Explorer öffnen<br> **\\\10.71.13.20\HTMLShare**<br>Nur Benutzer "samba" ist berechtigt | Nur Benutzer "samba" hat Zugriffsrechte |
| Samba | Berechtigung<br>FileShare | Windows Explorer öffnen<br> **\\\10.71.13.20\FileShare**<br>Nur Benutzer "samba2" ist berechtigt | Nur Benutzer "samba2" hat Zugriffsrechte |



**Webserver**  
![](/Images/Webserver.png)

**Samba Freigabe**  
![](/Images/SambaShare.png)  
***

## Troubleshooting