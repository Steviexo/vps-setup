# ⚙️ PHP-Tuning für Nextcloud

Dieses Dokument beschreibt die notwendigen Anpassungen in der PHP-Konfiguration zur Optimierung der Leistung und Kompatibilität für eine selbstgehostete Nextcloud-Instanz (Debian 12, PHP 8.2, Apache).

---

## ✅ Ziel

* Speicherlimit und OPcache anpassen
* Stabilen Betrieb sicherstellen
* Warnhinweise in Nextcloud-Übersicht vermeiden

---

## 📂 PHP-Konfigurationsdatei finden

```bash
php -i | grep "Loaded Configuration File"
```

> Erwartete Ausgabe (Apache-Modul): `/etc/php/8.2/apache2/php.ini`

---

## 🔧 Empfohlene Werte setzen

Bearbeiten:

```bash
sudo nano /etc/php/8.2/apache2/php.ini
```

### Wichtige Parameter:

```ini
memory_limit = 512M
upload_max_filesize = 1024M
post_max_size = 1024M
max_execution_time = 360
max_input_time = 360

[opcache]
opcache.enable=1
opcache.enable_cli=1
opcache.interned_strings_buffer=16
opcache.max_accelerated_files=10000
opcache.memory_consumption=128
opcache.save_comments=1
opcache.revalidate_freq=1
```

> 🔍 Suche die Zeilen mit `Ctrl + W` (z. B. `interned_strings_buffer`)

Speichern mit `Ctrl + O`, schließen mit `Ctrl + X`

---

## 🔁 Apache neu starten

```bash
sudo systemctl restart apache2
```

---

## 🔎 Prüfung mit `phpinfo()` (optional)

Erstelle temporäre Datei:

```bash
echo '<?php phpinfo(); ?>' > /var/www/nextcloud/info.php
```

Dann im Browser aufrufen: `https://nextcloud.example.de/info.php`

Am Ende löschen:

```bash
rm /var/www/nextcloud/info.php
```

---

## 🔄 PHP-CLI ebenfalls anpassen (optional für occ-CLI)

Pfad: `/etc/php/8.2/cli/php.ini`
→ gleiche Werte wie oben setzen
→ `php -i` zeigt dann die aktiven CLI-Werte

---

## 📌 Hinweise

* Änderungen gelten nur für Webserver, **nicht automatisch für CLI!**
* OPcache-Werte verhindern Performanceprobleme und Warnungen im Admin-Panel
* Nach PHP-Update können Werte zurückgesetzt werden → regelmäßig prüfen

---

## 🔗 Weiterführend

* [Nextcloud Systemanforderungen](https://docs.nextcloud.com/server/latest/admin_manual/installation/system_requirements.html)
* [Apache-Konfiguration](apache-config.md)
* [SSL-Setup](ssl-setup.md)

---

✍ **Letzte Aktualisierung:** Mai 2025
