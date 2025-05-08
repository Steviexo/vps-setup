# 🌐 Apache-Konfiguration für Nextcloud

Dieses Dokument beschreibt die Konfiguration von Apache2 zur Bereitstellung einer Nextcloud-Instanz unter Debian 12. Die Konfiguration ist für HTTPS über Let's Encrypt vorbereitet und unterstützt saubere URLs (mod\_rewrite).

---

## ✅ Voraussetzungen

* Apache2 installiert (`apt install apache2`)
* Nextcloud liegt unter `/var/www/nextcloud`
* Domain/Subdomain zeigt auf VPS (DNS korrekt eingerichtet)

---

## 🔧 Apache-Site-Datei erstellen

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

    RewriteEngine on
    RewriteCond %{SERVER_NAME} =nextcloud.example.de
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

> Ersetze `nextcloud.example.de` durch deine tatsächliche Domain.

---

## 🧩 Aktivierung der Konfiguration

```bash
a2ensite nextcloud.conf
a2enmod rewrite headers env dir mime ssl
systemctl reload apache2
```

---

## 🔐 SSL-Erweiterung (separate Datei nach Certbot)

Pfad: `/etc/apache2/sites-available/nextcloud-le-ssl.conf`

```apacheconf
<IfModule mod_ssl.c>
<VirtualHost *:443>
    ServerName nextcloud.example.de
    DocumentRoot /var/www/nextcloud

    <Directory /var/www/nextcloud/>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined

    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/nextcloud.example.de/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/nextcloud.example.de/privkey.pem

    # Sicherheit: HSTS-Header setzen
    Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
</VirtualHost>
</IfModule>
```

> Hinweis: Diese Datei wird i. d. R. automatisch durch `certbot --apache` erstellt.

---

## 📌 Tipps & Kontrolle

* Überprüfen, ob Apache korrekt läuft:

```bash
systemctl status apache2
```

* Testen, ob Ports offen sind:

```bash
ss -tuln | grep ':80\|:443'
```

* Logs bei Fehlern:

```bash
tail -f /var/log/apache2/*.log
```

---

## 🔗 Weiterführend

* [Nextcloud-Installation](nextcloud-installation.md)
* [SSL & Certbot](ssl-setup.md)
* [PHP-Optimierung](php-tuning.md)

---

✍ **Letzte Aktualisierung:** Mai 2025
