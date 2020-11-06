# Docker DNS

Es gibt den legacy Weg über die `--link` Flag und die aktuelle Variante mittels DNS.
Die Link Flag sorgt beim starten eines Containers dafür, dass unter `/etc/hosts` ein Eintrag für den entsprechenden anderen Container angelegt wird.
Dies funktioniert allerdings nur einseitig, da der andere Container schon gestartet sein muss.

Die bessere Variante ist es mit `docker network create dnsnet --subnet 192.168.54.0/24 --gateway 192.168.54.1` ein Docker Netzwerk im Bridge Modus anzulegen.
Diesem können nun mit `--network dnsnet` Container hinzugefügt werden.
Jeder Container kann mit der Flag `--network-alias` einen eigenen Netzwerknamen bekommen.

Z.B. können folgende Container erstellt werden:

```bash
docker run --rm -itd --name alpinedns1 --network dnsnet --network-alias alpinedns1 alpine &&
docker run --rm -itd --name alpinedns2 --network dnsnet --network-alias alpinedns2 alpine
```

Wenn nun eine Shell in den Containern geöffnet wird kann mit dem definierten `network-alias` gepingt werden.
Entsprechender Alias ist auch nicht in der `/etc/hosts` Datei zu finden.
