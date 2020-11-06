# Docker Bridge Demo

## Vorraussetzungen

Das Nginx Docker Image muss gebaut werden.
Dabei handelt es sich nur um ein Nginx Image, welches um den Befehl ping und curl erweitert wurde.

```bash
docker build -f Dockerfile -t mnginx .
```

## Vorführung

`docker network ls` zeigt alle vorhanden Docker Netzwerke an.
Mit `ifconfig` kann sich docker0 angezeigt werden lassen.
Eine Bridge, welche das Docker Subnetz (172.17.0.0/16) mit dem Host verbindet.

Dabei hat die Bridge die Adresse 172.17.0.1.

Nun kann mit `docker run --rm -d --name webserver1 mnginx` ein Nginx Container gestartet werden.
Mittels `docker ps` kann der laufende Container angezeigt werden.
Dabei ist zu erkennen, dass der Port 80 exposed wurde.
Wenn man nun `docker inspect webserver1` durchführt sieht man im Bereich Networks das Bridge Netzwerk.
Das Defaultgateway ist dabei die IP der Bridge.
Außerdem ist die IP des Containers zu erkken z.B. 172.17.0.2.

Dieser IP kann angepingt, gecurlt oder im Browser aufgerufen werden.
Nginx sollte einen dann begrüßen.

---

Docker bietet die Möglichkeit den Port auf den lokalen Host zu mappen mittels der Flag `-p`.
Mit `docker run --rm -d -p 8080:80 --name webserver2 mnginx` kann der Port 80 des Containers auf den Port 8080 des Hosts gemappt werden.

---

Da die beiden Container im gleichem Subnetz sind können sie auch miteinander kommunizieren.
`docker exec -it webserver1 /bin/bash` ermöglicht es die Konsole innerhalb des Containers aufzurufen.
Dort kann z.B. der zweite Webserver (172.17.0.3) angepingt werden.
