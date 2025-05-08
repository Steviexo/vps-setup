# 💾 VPS-Backup-Skript für Nextcloud

Dieses Dokument beschreibt das Bash-Skript zur automatisierten Sicherung einer selbstgehosteten Nextcloud-Instanz auf einem Debian-VPS. Das Skript wird regelmäßig per Cron ausgeführt und legt lokale Backups der Konfiguration, Datenbank und (optional) Nutzerdaten an.

---

## ✅ Zielsetzung

* Tägliche oder wöchentliche Sicherung auf dem VPS
* Backup der Nextcloud-Datenbank (MariaDB)
* Archivierung der Konfigurations- und (optional) Nutzerdaten
* Vorbereitung für automatisierten Pull durch das NAS

---

## 🧱 Voraussetzungen

| Komponente            | Details                                          |
| --------------------- | ------------------------------------------------ |
| Nextcloud-Verzeichnis | `/var/www/nextcloud`                             |
| Datenbank             | MariaDB, Benutzer `nextclouduser`                |
| Speicherort           | `/root/vps-backup/<Datum>`                       |
| Rechte                | Root oder Benutzer mit Schreibrechten in `/root` |

---

## 📜 Skriptinhalt

Pfad: `/usr/local/bin/nextcloud-vps-backup.sh`

```bash
#!/bin/bash
# VPS Nextcloud Backup-Skript

BACKUP_DIR="/root/vps-backup"
DATE=$(date +"%Y-%m-%d")
DB_USER="nextclouduser"
DB_PASS="HIER_DEIN_PASSWORT"
NEXTCLOUD_DIR="/var/www/nextcloud"

mkdir -p "$BACKUP_DIR/$DATE"

# 1. Datenbank sichern
mysqldump -u "$DB_USER" -p"$DB_PASS" nextcloud > "$BACKUP_DIR/$DATE/db.sql"

# 2. Konfiguration archivieren
tar -czf "$BACKUP_DIR/$DATE/config.tar.gz" "$NEXTCLOUD_DIR/config"

# 3. Optional: Nutzerdaten archivieren
tar -czf "$BACKUP_DIR/$DATE/data.tar.gz" "$NEXTCLOUD_DIR/data"

# 4. Aufräumen: ältere Backups löschen (älter als 7 Tage)
find "$BACKUP_DIR" -maxdepth 1 -type d -mtime +7 -exec rm -rf {} \;
```

> 🔐 Hinweis: Achte darauf, das Skript nur für Benutzer mit root-Rechten zugänglich zu machen (`chmod 700`).

---

## 🕒 Automatisierung per Cron

```bash
sudo crontab -e
```

Beispiel-Eintrag für sonntags 03:30 Uhr:

```bash
30 3 * * 0 /usr/local/bin/nextcloud-vps-backup.sh
```

---

## 📦 Backup-Inhalte

| Datei           | Inhalt                                            |
| --------------- | ------------------------------------------------- |
| `db.sql`        | Datenbank-Dump (MySQL)                            |
| `config.tar.gz` | Konfigurationsverzeichnis der Instanz             |
| `data.tar.gz`   | Nutzerdaten (optional, bei Platzbedarf abwählbar) |

---

## 🔗 Weiterführend

* [VPS Pull-Backup auf NAS](https://github.com/Steviexo/nas-setup/blob/main/docs/backup/vps-nas-pull.md)
* [Nextcloud-Installation](nextcloud-installation.md)
* [PHP-Tuning](php-tuning.md)

---

✍ **Letzte Aktualisierung:** Mai 2025
