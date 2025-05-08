# ☁️ Nextcloud-Installation auf Netcup VPS

Dieses Dokument beschreibt die manuelle Installation von Nextcloud auf einem Debian-12-VPS bei Netcup. Dabei kommen Apache2, MariaDB und PHP 8.2 zum Einsatz.

---

## ✅ Voraussetzungen

* Netcup VPS mit Debian 12
* Root-Zugang oder Benutzer mit `sudo`-Rechten (z. B. `stevie`)
* Domain/Subdomain eingerichtet und auf VPS-IP geroutet
* A-Record in DNS (z. B. `nextcloud.example.de` → VPS-IP)
* Apache, PHP und MariaDB installiert (siehe separate Doku)

---

## 🧱 Vorbereitung

### Benutzerwechsel (falls nötig):

```bash
ssh stevie@<VPS-IP>
sudo -i
```

### System aktualisieren:

```bash
apt update && apt upgrade -y
```

---

## 📦 Nextcloud herunterladen & entpacken

```bash
cd /tmp
wget https://download.nextcloud.com/server/releases/latest.tar.bz2
wget https://download.nextcloud.com/server/releases/latest.tar.bz2.sha256
sha256sum -c latest.tar.bz2.sha256
```

```bash
tar -xvjf latest.tar.bz2
mv nextcloud /var/www/
chown -R www-data:www-data /var/www/nextcloud
```

---

## 🌐 VirtualHost vorbereiten (siehe `apache-config.md`)

Pfad: `/etc/apache2/sites-available/nextcloud.conf`

```apacheconf
<VirtualHost *:80>
    ServerName nextcloud.example.de
    DocumentRoot /var/www/nextcloud

    <Directory /var/www/nextcloud/>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined
</VirtualHost>
```

Aktivieren:

```bash
a2ensite nextcloud.conf
a2enmod rewrite
systemctl reload apache2
```

---

## 🔐 HTTPS aktivieren (nach Abschluss siehe `ssl-setup.md`)

```bash
apt install certbot python3-certbot-apache -y
certbot --apache
```

---

## 🛠️ MariaDB vorbereiten (Beispiel)

```bash
mariadb -u root -p
```

```sql
CREATE DATABASE nextcloud;
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'SICHERES_PASSWORT';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 🚀 Web-Setup ausführen

1. Browser öffnen: `https://nextcloud.example.de`
2. Benutzerkonto erstellen (Admin)
3. Datenbankzugang eintragen
4. Verzeichnisprüfung läuft automatisch
5. Installation abschließen

---

## 🔧 Nachbearbeitung

* Cron statt AJAX aktivieren:

```bash
sudo -u www-data crontab -e
*/5 * * * * php -f /var/www/nextcloud/cron.php
```

* Datei-Cache optimieren
* PHP-OPcache prüfen (siehe `php-tuning.md`)

---

## 📌 Weiterführend

* [Apache-Konfiguration](apache-config.md)
* [PHP-Tuning](php-tuning.md)
* [SSL & Sicherheit](ssl-setup.md)
* [Backup-Skript](backup-script.md)

---

✍ **Letzte Aktualisierung:** Mai 2025
