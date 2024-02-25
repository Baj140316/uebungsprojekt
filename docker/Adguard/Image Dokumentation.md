Hier ist die Übersetzung:

---

Schnellstart
Holen Sie sich das Docker-Image

Mit diesem Befehl wird die neueste stabile Version heruntergeladen:

docker pull adguard/adguardhome

Erstellen Sie Verzeichnisse für persistente Konfiguration und Daten

Das Image stellt zwei Volumes für die Daten- und Konfigurationspersistenz bereit. Sie sollten ein Datenverzeichnis auf einem geeigneten Volume auf Ihrem Host-System erstellen, z.B. /my/own/workdir, und ein Konfigurationsverzeichnis auf einem geeigneten Volume auf Ihrem Host-System erstellen, z.B. /my/own/confdir.
Container erstellen und ausführen

Verwenden Sie den folgenden Befehl, um einen neuen Container zu erstellen und AdGuard Home auszuführen:

docker run --name adguardhome\
    --restart unless-stopped\
    -v /my/own/workdir:/opt/adguardhome/work\
    -v /my/own/confdir:/opt/adguardhome/conf\
    -p 53:53/tcp -p 53:53/udp\
    -p 67:67/udp -p 68:68/udp\
    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp\
    -p 853:853/tcp\
    -p 853:853/udp\
    -p 5443:5443/tcp -p 5443:5443/udp\
    -p 6060:6060/tcp\
    -d adguard/adguardhome

Nun können Sie den Browser öffnen und zu http://127.0.0.1:3000/ navigieren, um Ihren AdGuard Home-Dienst zu steuern.

Vergessen Sie nicht, Ihre eigenen Daten- und Konfigurationsverzeichnisse zu verwenden!

Port-Zuordnungen, die Sie benötigen könnten:

    -p 53:53/tcp -p 53:53/udp: einfaches DNS.

    -p 67:67/udp -p 68:68/tcp -p 68:68/udp: hinzufügen, wenn Sie AdGuard Home als DHCP-Server verwenden möchten.

    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp: hinzufügen, wenn Sie das Admin-Panel von AdGuard Home verwenden möchten und AdGuard Home als HTTPS/DNS-over-HTTPS-Server ausführen.

    -p 853:853/tcp: hinzufügen, wenn Sie AdGuard Home als DNS-over-TLS-Server ausführen möchten.

    -p 853:853/udp: hinzufügen, wenn Sie AdGuard Home als DNS-over-QUIC-Server ausführen möchten.

    -p 5443:5443/tcp -p 5443:5443/udp: hinzufügen, wenn Sie AdGuard Home als DNSCrypt-Server ausführen möchten.

    -p 6060:6060/tcp: Debugging-Profile.

Client-IPs

Wenn Sie möchten, dass AdGuardHome die ursprünglichen Client-IPs sieht, anstatt etwas wie 172.17.0.1, sollten Sie --network host zur Liste der Optionen hinzufügen.
Container steuern

    Start: docker start adguardhome

    Stop: docker stop adguardhome

    Entfernen: docker rm adguardhome

Update auf eine neuere Version

    Holen Sie sich die neue Version von Docker Hub:

    docker pull adguard/adguardhome

Stoppen und Entfernen des derzeit ausgeführten Containers (vorausgesetzt, der Container heißt adguardhome):

docker stop adguardhome
docker rm adguardhome

    Erstellen und Starten des Containers mit dem neuen Image mit dem Befehl aus dem vorherigen Abschnitt.

Entwicklungsversionen ausführen

Wenn Sie ganz vorn sein möchten, möchten Sie vielleicht das Image von den Edge- oder Beta-Tags ausführen. Um es zu verwenden, ersetzen Sie einfach adguard/adguardhome durch adguard/adguardhome:edge oder adguard/adguardhome:beta in jedem Befehl aus dem Schnellstart. Zum Beispiel:

docker pull adguard/adguardhome:edge

Zusätzliche Konfiguration

Beim ersten Start wird eine Datei mit den Standardwerten namens AdGuardHome.yaml erstellt. Sie können die Datei bearbeiten, solange Ihr AdGuard Home-Container nicht läuft. Andernfalls gehen Änderungen an der Datei verloren, weil das laufende Programm sie überschreibt.

Die Einstellungen werden im YAML-Format gespeichert. Die Dokumentation, die alle konfigurierbaren Parameter und ihre Werte beschreibt, ist auf dieser Seite verfügbar.
HEALTHCHECK

Zwischen v0.107.27 und v0.107.33 verwendete das Image den von Docker bereitgestellten Healthcheck-Mechanismus. Dies führte zu vielen Problemen und wurde in v0.107.34 entfernt. Siehe Probleme #5711, #5713 und Diskussion #5939.

Wenn Sie einen Healthcheck-Mechanismus benötigen, ist es besser, Ihr eigenes Image für Ihre Konfiguration anzupassen. Implementierungen können den speziellen Domainnamen healthcheck.adguardhome.test verwenden, in der Erwartung, dass er sich in eine NODATA-Antwort auflöst. Dies setzt Einschränkungen für die Verwendung dieses bestimmten Namens voraus, sodass die Angabe in der blocked_hosts-Array unter dem dns-Abschnitt der Konfigurationsdatei den Healthcheck unterbrechen wird. Die allowed_clients- und disallowed_clients-Eigenschaften sollten auch die IP des Healthcheck-Clients zulassen.
DHCP-Server

Wenn Sie den DHCP-Server von AdGuardHome verwenden möchten, sollten Sie das Argument --network host übergeben, wenn Sie den Container erstellen:

docker run --name adguardhome --network host ...

Diese Option weist Docker an, das Netzwerk des Hosts anstelle eines docker-gebrückten Netzwerks zu verwenden. Beachten Sie, dass in diesem Fall keine Portzuordnung mit -p erforderlich ist.

Eine Anmerkung aus der Docker-Dokumentation:

    Der Host-Netzwerk-Treiber funktioniert nur auf Linux-Hosts und wird nicht von Docker Desktop für Mac, Docker Desktop für Windows oder Docker EE für Windows Server unterstützt.

gelöst

Wenn Sie versuchen, AdGuardHome auf einem System auszuführen, auf dem der resolved-Daemon gestartet ist, wird Docker fehlschlagen, um auf Port 53 zu binden, weil der resolved-Daemon auf 127.0.0.53:53 hört. So deaktivieren Sie DNSStubListener auf Ihrem Rechner:

    De

aktivieren Sie DNSStubListener und aktualisieren Sie die DNS-Serveradresse. Erstellen Sie eine neue Datei, /etc/systemd/resolved.conf.d/adguardhome.conf (Erstellen des Verzeichnisses /etc/systemd/resolved.conf.d, wenn erforderlich) und fügen Sie den folgenden Inhalt hinzu:

    [Resolve]
    DNS=127.0.0.1
    DNSStubListener=no

    Die Angabe von 127.0.0.1 als DNS-Serveradresse ist notwendig, da sonst der Nameserver 127.0.0.53 wäre, was ohne DNSStubListener nicht funktioniert.

    Aktivieren Sie eine neue resolv.conf-Datei:

    mv /etc/resolv.conf /etc/resolv.conf.backup
    ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf

    Stoppen Sie DNSStubListener:

    systemctl reload-or-restart systemd-resolved

---
