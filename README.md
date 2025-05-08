# 📖 VPS-Setup – Dokumentation & Best Practices

Willkommen im Repository **vps-setup**! Dieses Repository dient als **Wissenssammlung und Best Practices zur Einrichtung eines selbstverwalteten VPS**. Hier dokumentiere ich meine Erfahrungen, um:

✅ **Mein eigenes Wissen zu strukturieren & dokumentieren**
✅ **Anderen (auch nicht-tech-affinen) Nutzern verständliche Anleitungen bereitzustellen**
✅ **Meine Fortschritte als zukünftige Admin sichtbar zu machen**

Dieses Repository wächst mit meinen Erfahrungen und ist eine zentrale Anlaufstelle für die Konfiguration meines Nextcloud-VPS. Hier sammle ich bewährte Methoden, hilfreiche Anleitungen und Lösungswege für typische Aufgaben wie Absicherung, Webhosting und Backup.

## 🖥 Technische Ausstattung

Dieses Repository basiert auf einem **Netcup VPS 500 G11s** mit folgender Ausstattung:

* **Provider**: Netcup
* **Modell**: VPS 500 G11s
* **Prozessor**: 4 vCore (x86)
* **RAM**: 4 GB DDR ECC
* **Speicher**: 128 GB SSD + optional Local Block Storage
* **Betriebssystem**: Debian 12 (Bookworm)
* **Zugriff**: SSH mit Public-Key-Authentifizierung

## 📂 Verzeichnisstruktur & Unterthemen

```
vps-setup/
├── docs/                       # Dokumentation zu diesem Repository
│   ├── README.md               # Einführung und Übersicht
│   ├── nextcloud-installation.md  # Installation von Nextcloud auf dem VPS
│   ├── apache-config.md           # Einrichtung des Webservers
│   ├── ssl-setup.md               # HTTPS + HSTS über Let's Encrypt
│   ├── php-tuning.md              # Optimierungen für Nextcloud (z. B. OPcache)
│   └── backup-script.md           # VPS-Backup-Skript + Ablauf
├── scripts/                    # Bash-Skripte zur Automatisierung
│   └── nextcloud-vps-backup.sh
├── canvas/                     # Exporte aus ChatGPT-Canvas zur Nachverfolgung
│   └── nextcloud-vps-setup.md
├── LICENSE
├── CONTRIBUTING.md
└── README.md                   # Diese Datei
```

## 📌 Enthaltene Themen

### 🔹 **VPS-Einrichtung & Nextcloud**

* **[Nextcloud-Installation](docs/nextcloud-installation.md)**: Manuelle Einrichtung über Apache, MariaDB und PHP
* **[SSL & Sicherheit](docs/ssl-setup.md)**: Let's Encrypt, HSTS, SSH-Hardening
* **[PHP-Tuning](docs/php-tuning.md)**: Speicherlimit, OPcache, Performance

### 🔹 **Backup & Recovery**

* **[VPS-Backup-Skript](docs/backup-script.md)**: Vollautomatisierte Sicherung per Cron & rsync
* **Pull vom NAS**: Aufgabenplanung auf dem NAS per DSM
* **Referenz zur Hyper Backup-Strategie** im [nas-setup](https://github.com/Steviexo/nas-setup)

### 🚧 **In Arbeit / Geplante Inhalte**

* Automatische Sicherheitsupdates (`unattended-upgrades`)
* Fail2ban + Firewall (UFW oder nftables)
* Wiederherstellungstest in Testumgebung

---

## 📝 Warum dieses Repository?

Ich nutze GitHub primär als **persönliches Dokumentations- und Wissensmanagement-Tool**. Dieses Repository hilft mir dabei, …

* meine **VPS-Strategie & Best Practices** festzuhalten
* verständliche, strukturierte Anleitungen für andere zu schaffen
* meine **Kompetenz im Bereich Linux-/Serveradministration** sichtbar zu machen

Falls du Fragen hast oder Feedback geben möchtest, freue ich mich über deine Nachricht! 😊

---

## 🚀 Verwendung

Hier findest du alle wichtigen Informationen zur Nutzung dieses Repositories.

* **Nextcloud-Installation**: Lies die [Installationsanleitung](docs/nextcloud-installation.md) für den Einstieg.
* **SSL & Sicherheit**: Die Absicherung wird in [ssl-setup.md](docs/ssl-setup.md) beschrieben.
* **Backup-Skript**: Details unter [backup-script.md](docs/backup-script.md).

## 🤝 Mitwirken

Falls du Änderungen oder Verbesserungen beisteuern möchtest, lies die [CONTRIBUTING.md](CONTRIBUTING.md).

## 🔗 Weiterführende Informationen

* [Nextcloud-Dokumentation](https://docs.nextcloud.com/)
* [Debian Serverhandbuch](https://wiki.debian.org/)
* [Netcup Wiki](https://www.netcup-wiki.de/wiki/)
* [Projekt-Template](https://github.com/steviexo/project-template)

---

✍ **Letzte Aktualisierung:** Mai 2025
