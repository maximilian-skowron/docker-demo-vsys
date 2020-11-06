# Netzwerk Treiber

## Bridge

Dieser wurde in den vorherigen Aufgaben besprochen.

## None

Mit `docker network inspect none` kann man sich die genaue Definitions anzeigen lassen.
Dieser Treiber sorgt dafür, dass ein Container nur einen Localhost besitzt und sonst keinerlei Verbindungen.
Nachdem man `docker run --rm -itd --name alpine --network none apline:latest && docker exec -it alpine /bin/sh` ausgeführt hat kann mit `ip a` dieser Sachverhalt aufgezeigt werden.

Wenn man den erstellten Container zusätzlich mit `inspect` untersucht zeigt sich, dass kein Default Gateway vorhanden ist.

## Host

Beim Host Treiber wird der Container im Netzwerk des lokalen Hosts ausgeführt.
Dabei hat der Container zugriff auf alle Netzwerk Adapter des Hosts.

```
docker run --rm -itd --name nginx --network host nginx:latest
docker run -itd --name nginx2 --network host nginx:latest
```

Bei zweiten Befehl tritt ein Fehler auf.
Dieser kann mit `docker logs nginx2` aufgezeigt werden.
Dort wird berichtet, dass es nicht möglich war den Port 80 zu binden.
Das ist klar, da schon ein andere Container (nginx) diesen Port gebunden hat.
Dies kann auch validiert werden, indem man einen Browser öffnet und dort localhost:80 eingibt.

## Macvlan

Macvlan sorgt dafür, dass jeder Container eine eigene Macadresse zur Verfügung bekommt.
Dazu muss ein Netzwerk erstellt werden, was dem Netzwerk des Hosts entspricht.
Der Host hat z.B. als eth0 192.168.122.188/24, dann lautet der Befehl `docker network create -d macvlan --subnet=192.168.122.0/24` --gateway=192.168.122.1 -o parent=eth0 macvlan-net1`.

Nun können mit der `--network macvlan-net1` Container gestartet werden.
Wenn zwei Container gestartet werden kann innerhalb eines der beiden `arping` installiert werden.
Damit können ARP Nachrichten an die MAC Adresse geschickt werden und die Verbindung auf Layer 2 (ISO/OSI) getestet werden.

```
arping 192.168.122.x <- Adresse eines Containers
```
