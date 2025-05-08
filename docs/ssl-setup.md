# 🔐 SSL-Setup für Nextcloud mit Let's Encrypt

Dieses Dokument beschreibt die Absicherung einer selbstgehosteten Nextcloud-Instanz auf einem Apache-Webserver via HTTPS mit **Let's Encrypt**. Die Anleitung basiert auf Debian 12 und Certbot.

---

## ✅ Voraussetzungen

* Domain/Subdomain zeigt per A-Record auf die VPS-IP (z. B. `nextcloud.example.de`)
* Apache2 läuft und ist für Port 80 konfiguriert (siehe `apache-config.md`)
* Nextcloud ist über HTTP erreichbar

---

## 📦 Certbot installieren

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
```

---

## 🔧 SSL-Zertifikat generieren & Apache konfigurieren

```bash
sudo certbot --apache
```

Du wirst nach deiner Domain (z. B. `nextcloud.example.de`) und einer Mailadresse gefragt. Certbot:

* generiert das Zertifikat
* konfiguriert Apache automatisch (HTTPS + Redirect von HTTP)
* erstellt `/etc/apache2/sites-available/nextcloud-le-ssl.conf`

---

## 🧩 HSTS aktivieren (manuell ergänzen)

In der Datei `nextcloud-le-ssl.conf`, Abschnitt `<VirtualHost *:443>`, ergänzen:

```apacheconf
Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains"
```

Danach:

```bash
sudo systemctl reload apache2
```

---

## 🔁 Automatische Verlängerung testen

Let's Encrypt-Zertifikate sind 90 Tage gültig. Test der automatischen Verlängerung:

```bash
sudo certbot renew --dry-run
```

Der Cronjob wird bei der Installation automatisch eingerichtet. Du kannst ihn manuell ausführen oder in Logdateien prüfen:

```bash
cat /var/log/letsencrypt/letsencrypt.log
```

---

## 🔍 Test & Validierung

* Zugriff auf: `https://nextcloud.example.de`
* Browser zeigt „Verbindung sicher“ + gültiges Zertifikat
* Header testen:

```bash
curl -I https://nextcloud.example.de | grep -i strict
```

→ Ausgabe sollte beinhalten: `Strict-Transport-Security: max-age=...`

---

## 📌 Sicherheitshinweise

* Nur Port 443 offen lassen (Firewall prüfen)
* Passwortauthentifizierung in SSH deaktivieren (Public-Key-Login empfohlen)
* Keine unnötigen Apache-Module aktiv lassen (nur: rewrite, ssl, headers, env, mime, dir)

---

## 🔗 Weiterführend

* [Nextcloud-Installation](nextcloud-installation.md)
* [Apache-Konfiguration](apache-config.md)
* [PHP-Tuning](php-tuning.md)

---

✍ **Letzte Aktualisierung:** Mai 2025
