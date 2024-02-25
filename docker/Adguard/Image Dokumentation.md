Diese Dockerfile beschreibt den Aufbau eines Docker-Images für AdGuard Home, einer Anwendung zur Blockierung von Anzeigen und Trackern auf Netzwerkebene. Hier ist eine Beschreibung:

---

Dieses Docker-Image basiert auf Alpine Linux Version 3.18 und enthält AdGuard Home, einen DNS-Server zur Blockierung von Anzeigen und Trackern auf Netzwerkebene.

Es enthält folgende Labels:

- **maintainer**: Die E-Mail-Adresse des AdGuard-Teams für Wartungszwecke.
- **org.opencontainers.image.authors**: Die Autoren des Images.
- **org.opencontainers.image.created**: Das Erstellungsdatum des Images.
- **org.opencontainers.image.description**: Eine kurze Beschreibung des Images.
- **org.opencontainers.image.documentation**: Die URL zur Dokumentation des Images.
- **org.opencontainers.image.licenses**: Die Lizenz des Images.
- **org.opencontainers.image.revision**: Die Versionskontroll-Referenz des Images.
- **org.opencontainers.image.source**: Die Quellcode-URL des Images.
- **org.opencontainers.image.title**: Der Titel des Images.
- **org.opencontainers.image.url**: Die URL des Images.
- **org.opencontainers.image.vendor**: Der Hersteller des Images.
- **org.opencontainers.image.version**: Die Versionsnummer des Images.

Die Zertifikate werden aktualisiert, und die erforderlichen Pakete werden installiert. Außerdem werden Verzeichnisse für Konfiguration und Arbeit erstellt, und die Berechtigungen werden entsprechend angepasst.

Der AdGuard Home-Binärdatei wird die Fähigkeit gegeben, Ports unter 1024 zu binden.

Verschiedene Ports werden freigegeben, um den Dienst über verschiedene Protokolle zu erreichen, darunter DNS, DHCP, HTTP, HTTPS, DNS-over-TLS, DNS-over-QUIC und DNSCrypt.

Der Arbeitsverzeichnis wird auf das Arbeitsverzeichnis von AdGuard Home festgelegt.

Das Entrypoint wird auf die AdGuard Home-Binärdatei gesetzt, und die Standardausführung wird auf das Starten von AdGuard Home mit spezifizierter Konfigurationsdatei und Arbeitsverzeichnis gesetzt.

---
