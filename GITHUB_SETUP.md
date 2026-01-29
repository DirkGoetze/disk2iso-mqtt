# GitHub Repository Setup - Schritt-für-Schritt Anleitung

## ✅ Was wurde bereits erledigt

Das MQTT-Modul-Repository wurde lokal erstellt und ist bereit für GitHub:

- 📁 Ordner: `L:\clouds\onedrive\Dirk\projects\disk2iso-mqtt`
- 📝 17 Dateien (lib, lang, conf, www, README, LICENSE, etc.)
- 🎯 Git-Repository initialisiert
- 💾 Commit erstellt: `48d4b3a`
- 🏷️ Tag erstellt: `v1.0.0`

---

## 📋 Nächste Schritte

### 1. GitHub Repository erstellen

1. Öffne GitHub: https://github.com/new
2. Fülle Formular aus:
   ```
   Repository name:     disk2iso-mqtt
   Description:         MQTT Integration Plugin für disk2iso - Home Assistant Support
   Visibility:          ☑ Public
   
   ⚠️ WICHTIG: NICHTS auswählen bei:
   - [ ] Add a README file
   - [ ] Add .gitignore
   - [ ] Choose a license
   
   (Wir haben diese Dateien bereits lokal!)
   ```
3. Klicke auf **"Create repository"**

---

### 2. Lokales Repository mit GitHub verbinden

Öffne PowerShell und führe folgende Befehle aus:

```powershell
# Navigiere zum Repository
cd L:\clouds\onedrive\Dirk\projects\disk2iso-mqtt

# Füge GitHub als Remote hinzu (ERSETZE URL MIT DEINEM REPO!)
git remote add origin https://github.com/DirkGoetze/disk2iso-mqtt.git

# Pushe Code zu GitHub
git branch -M master
git push -u origin master

# Pushe Tag
git push --tags
```

**Expected Output:**
```
Enumerating objects: 17, done.
Counting objects: 100%, done.
...
To https://github.com/DirkGoetze/disk2iso-mqtt.git
 * [new branch]      master -> master
 * [new tag]         v1.0.0 -> v1.0.0
```

---

### 3. Repository-Einstellungen konfigurieren

#### A) Topics hinzufügen (für bessere Auffindbarkeit)

1. Gehe zur **Repository-Hauptseite**: https://github.com/DirkGoetze/disk2iso-mqtt
2. Schaue **rechts oben** auf die **"About"-Sektion**
3. Klicke auf das **⚙️ Zahnrad-Icon** neben "About"
4. Im Dialog unter **"Topics"** füge hinzu:
   ```
   mqtt
   home-assistant
   home-automation
   disk2iso
   plugin
   python
   bash
   ```
5. Klicke **"Save changes"**

#### B) GitHub Actions aktivieren

1. Gehe zu: **Settings → Actions → General**
2. Unter **"Actions permissions"**:
   - ☑ **Allow all actions and reusable workflows**
3. Speichern

---

### 4. Erstes Release erstellen

#### Option A: Via GitHub UI (einfach)

1. Gehe zu: https://github.com/DirkGoetze/disk2iso-mqtt/releases
2. Klicke auf **"Draft a new release"**
3. Fülle Formular aus:
   ```
   Choose a tag: v1.0.0
   Release title: MQTT Module v1.0.0 - Initial Release
   
   Describe this release:
   ## 🚀 MQTT Module v1.0.0 - Initial Release
   
   Erste stabile Version des MQTT-Moduls für disk2iso.
   
   ### Features
   - Home Assistant Auto-Discovery
   - Echtzeit-Status-Updates
   - Fortschritts-Tracking
   - Web-UI Widgets mit Auto-Save
   - CLI-Interface für Scripting
   
   ### Installation
   Download `mqtt-module.zip` unten und folge der Anleitung im README.
   ```
4. Klicke auf **"Publish release"**

**GitHub Actions wird automatisch:**
- `mqtt-module.zip` erstellen
- SHA256-Checksum generieren
- Als Release-Artifact hochladen

#### Option B: Via CLI (fortgeschritten)

```bash
# Mit GitHub CLI (gh)
gh release create v1.0.0 --title "MQTT Module v1.0.0 - Initial Release" --notes "Erste stabile Version"
```

---

### 5. README im Haupt-Repository anpassen

Jetzt muss das Haupt-Repository (`disk2iso`) auf das neue Modul-Repository verweisen.

**Datei:** `L:\clouds\onedrive\Dirk\projects\disk2iso\README.md`

Füge folgenden Abschnitt hinzu (z.B. nach "Features"):

```markdown
## 🔌 Verfügbare Module

disk2iso unterstützt optionale Module für erweiterte Funktionen:

| Modul | Status | Beschreibung | Installation |
|-------|--------|--------------|--------------|
| [MQTT](https://github.com/DirkGoetze/disk2iso-mqtt) | ✅ Stabil | Home Assistant Integration | [Download](https://github.com/DirkGoetze/disk2iso-mqtt/releases/latest) |
| TMDB | 🚧 Geplant | Film-Metadaten | - |
| MusicBrainz | 🚧 Geplant | Audio-CD Metadaten | - |

### MQTT-Modul installieren

```bash
# Automatisch
curl -L https://github.com/DirkGoetze/disk2iso-mqtt/releases/latest/download/mqtt-module.zip -o /tmp/mqtt.zip
cd /opt/disk2iso
sudo unzip /tmp/mqtt.zip
sudo systemctl restart disk2iso-web
```

Oder via Web-UI: **Einstellungen → Module → MQTT → Installieren**

Mehr Info: [MQTT-Modul Dokumentation](https://github.com/DirkGoetze/disk2iso-mqtt)
```

**Commit & Push:**
```bash
cd L:\clouds\onedrive\Dirk\projects\disk2iso
git add README.md
git commit -m "docs: Add MQTT module to available modules list"
git push
```

---

### 6. Verifizierung

Prüfe ob alles funktioniert:

1. **Repository sichtbar?**
   - https://github.com/DirkGoetze/disk2iso-mqtt

2. **README wird angezeigt?**
   - Badges funktionieren
   - Markdown korrekt gerendert

3. **Release vorhanden?**
   - https://github.com/DirkGoetze/disk2iso-mqtt/releases/tag/v1.0.0
   - `mqtt-module.zip` als Asset verfügbar

4. **GitHub Actions erfolgreich?**
   - https://github.com/DirkGoetze/disk2iso-mqtt/actions
   - Grüner Haken bei "Release MQTT Module"

---

## 📊 Repository-Übersicht

### Dateien im Repository

```
disk2iso-mqtt/
├── .github/
│   └── workflows/
│       └── release.yml          # GitHub Actions Workflow
├── conf/
│   └── libmqtt.ini             # Modul-Konfiguration
├── lang/
│   ├── libmqtt.de              # Deutsche Übersetzungen
│   ├── libmqtt.en              # Englische Übersetzungen
│   ├── libmqtt.es              # Spanische Übersetzungen
│   └── libmqtt.fr              # Französische Übersetzungen
├── lib/
│   └── libmqtt.sh              # MQTT-Modul Bash-Script
├── www/
│   ├── routes/
│   │   └── routes_mqtt.py      # Flask Blueprint
│   ├── static/js/widgets/
│   │   ├── mqtt.js             # Index-Widget JS
│   │   └── mqtt_config.js      # Config-Widget JS
│   └── templates/widgets/
│       ├── mqtt_widget.html    # Index-Widget HTML
│       └── mqtt_config_widget.html  # Config-Widget HTML
├── .gitignore
├── CHANGELOG.md                 # Versions-Historie
├── LICENSE                      # MIT License
├── README.md                    # Hauptdokumentation
└── VERSION                      # Versionsnummer (1.0.0)
```

### Git-Status

```bash
# Aktueller Commit
48d4b3a - feat: Initial MQTT Module Release v1.0.0

# Tag
v1.0.0 - Release v1.0.0 - Initial MQTT Module

# Remote
origin: https://github.com/DirkGoetze/disk2iso-mqtt.git (nach Setup)
```

---

## 🔄 Zukünftige Updates

### Neues Release erstellen

1. **Änderungen machen** (z.B. Bugfix)
2. **CHANGELOG.md updaten**
3. **VERSION updaten** (z.B. `1.0.1`)
4. **Commit & Tag:**
   ```bash
   git add .
   git commit -m "fix: Behebe XYZ Problem"
   git tag -a v1.0.1 -m "Release v1.0.1"
   git push && git push --tags
   ```
5. **GitHub Actions erstellt automatisch Release**

---

## ❓ Troubleshooting

### Problem: "authentication failed"

Wenn `git push` nach Passwort fragt:
```bash
# Nutze GitHub CLI (empfohlen)
gh auth login

# ODER Personal Access Token
# GitHub → Settings → Developer settings → Personal access tokens → Generate new token
# Scope: repo (full control)
```

### Problem: GitHub Actions Workflow startet nicht

1. Prüfe: **Settings → Actions → General**
2. "Allow all actions" muss aktiviert sein
3. Bei Problemen: **Actions Tab → Workflow manuell triggern**

### Problem: Release-Artifact fehlt

1. Gehe zu: **Actions → Latest Workflow Run**
2. Prüfe Logs auf Fehler
3. Häufigste Ursache: ZIP-Erstellung fehlgeschlagen

---

## 📝 Checkliste

- [ ] GitHub Repository erstellt
- [ ] Git Remote verbunden
- [ ] Code gepusht
- [ ] Tag gepusht
- [ ] Topics hinzugefügt
- [ ] GitHub Actions aktiviert
- [ ] Erstes Release erstellt
- [ ] README im Haupt-Repo aktualisiert
- [ ] Verifizierung durchgeführt

---

**Bei Fragen oder Problemen:** Erstelle ein Issue im Repository!

**Viel Erfolg! 🚀**
