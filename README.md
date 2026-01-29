# disk2iso MQTT Module

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/github/v/release/DirkGoetze/disk2iso-mqtt)](https://github.com/DirkGoetze/disk2iso-mqtt/releases)

MQTT Integration Plugin für [disk2iso](https://github.com/DirkGoetze/disk2iso) - ermöglicht Home Assistant Integration und Echtzeit-Monitoring.

## 🚀 Features

- **Home Assistant Auto-Discovery** - Automatische Sensor-Erkennung
- **Echtzeit-Status-Updates** - idle, copying, waiting, completed, error
- **Fortschritts-Tracking** - Prozent, MB, ETA
- **Medium-Informationen** - Label, Typ, Größe
- **Availability-Monitoring** - Online/Offline Status

## 📋 Voraussetzungen

- **disk2iso** >= v1.2.0 ([Installation](https://github.com/DirkGoetze/disk2iso))
- **mosquitto-clients** (MQTT Client-Tools)
- **MQTT Broker** (z.B. Mosquitto, Home Assistant)

## 📦 Installation

### Automatisch (empfohlen)

```bash
# Download neueste Version
curl -L https://github.com/DirkGoetze/disk2iso-mqtt/releases/latest/download/mqtt-module.zip -o /tmp/mqtt.zip

# Entpacken nach disk2iso
cd /opt/disk2iso
sudo unzip /tmp/mqtt.zip

# Service neu starten
sudo systemctl restart disk2iso-web
```

### Manuell

1. Download [neueste Release](https://github.com/DirkGoetze/disk2iso-mqtt/releases/latest)
2. Entpacke nach `/opt/disk2iso/`
3. Setze Berechtigungen: `sudo chown -R root:root /opt/disk2iso/`
4. Restart Service: `sudo systemctl restart disk2iso-web`

### Via Web-UI (ab v1.3.0)

1. Öffne disk2iso Web-UI
2. Gehe zu **Einstellungen → Module**
3. Klicke auf **MQTT → Installieren**

## ⚙️ Konfiguration

### Via Web-UI (empfohlen)

1. Öffne disk2iso Web-UI
2. Gehe zu **Einstellungen**
3. Aktiviere **MQTT Integration**
4. Konfiguriere MQTT Broker:
   - **Broker:** IP-Adresse deines MQTT Brokers
   - **Port:** 1883 (Standard)
   - **User/Password:** Optional, falls Auth erforderlich
5. Klicke auf **Verbindung testen**
6. Speichern (Auto-Save bei Fokus-Verlust)

### Via Konfigurationsdatei

Bearbeite `/opt/disk2iso/conf/disk2iso.conf`:

```bash
# MQTT Integration
MQTT_ENABLED=true
MQTT_BROKER="192.168.20.13"
MQTT_PORT=1883
MQTT_USER=""           # Optional
MQTT_PASSWORD=""       # Optional
```

Erweiterte Einstellungen in `/opt/disk2iso/conf/libmqtt.ini`:

```ini
[api]
topic_prefix=homeassistant/sensor/disk2iso
client_id=disk2iso-hostname
qos=0
retain=true
```

## 🏠 Home Assistant Integration

### Automatische Sensor-Erkennung

Das Modul nutzt Home Assistant Auto-Discovery. Sensoren werden automatisch erkannt:

- `sensor.disk2iso_status` - Aktueller Status
- `sensor.disk2iso_progress` - Fortschritt in %
- `sensor.disk2iso_disc_label` - Medium-Label
- `sensor.disk2iso_disc_type` - Medium-Typ (audio-cd, dvd-video, bluray)

### Manuelle YAML-Konfiguration

Falls Auto-Discovery nicht funktioniert:

```yaml
# configuration.yaml
sensor:
  - platform: mqtt
    name: "disk2iso Status"
    state_topic: "homeassistant/sensor/disk2iso/state"
    value_template: "{{ value_json.status }}"
    json_attributes_topic: "homeassistant/sensor/disk2iso/attributes"
    availability_topic: "homeassistant/sensor/disk2iso/availability"
```

Beispiel-Konfiguration: [homeassistant-configuration.yaml](https://github.com/DirkGoetze/disk2iso/blob/master/samples/homeassistant-configuration.yaml)

## 🔧 CLI-Interface

Das Modul bietet ein CLI-Interface für Scripting:

```bash
# Konfiguration exportieren (JSON)
/opt/disk2iso/lib/libmqtt.sh export-config

# Konfiguration updaten (JSON via stdin)
echo '{"mqtt_enabled": true, "mqtt_broker": "192.168.20.10"}' | \
  /opt/disk2iso/lib/libmqtt.sh update-config

# Verbindung testen
echo '{"broker": "192.168.20.10", "port": 1883}' | \
  /opt/disk2iso/lib/libmqtt.sh test-connection
```

## 📊 MQTT Topics

### Status Topic
```
homeassistant/sensor/disk2iso/state
{"status": "copying", "timestamp": "2026-01-29T21:30:00"}
```

### Attributes Topic
```
homeassistant/sensor/disk2iso/attributes
{
  "disc_label": "The Dark Knight",
  "disc_type": "bluray",
  "disc_size_mb": 45000,
  "progress_percent": 67,
  "progress_mb": 30150,
  "total_mb": 45000,
  "eta": "00:15:30",
  "filename": "The_Dark_Knight.mkv",
  "method": "makemkvcon",
  "container_type": "mkv"
}
```

### Progress Topic
```
homeassistant/sensor/disk2iso/progress
67
```

### Availability Topic
```
homeassistant/sensor/disk2iso/availability
online
```

## 🐛 Troubleshooting

### MQTT-Verbindung schlägt fehl

1. **Broker erreichbar?**
   ```bash
   mosquitto_pub -h 192.168.20.13 -p 1883 -t test -m "hello"
   ```

2. **Authentifizierung korrekt?**
   - Prüfe User/Password in Web-UI
   - Teste mit mosquitto_pub: `-u user -P password`

3. **Firewall-Regeln?**
   - Port 1883 muss offen sein

### Keine Sensoren in Home Assistant

1. **Auto-Discovery aktiviert?**
   ```yaml
   # configuration.yaml
   mqtt:
     discovery: true
     discovery_prefix: homeassistant
   ```

2. **MQTT Integration installiert?**
   - Home Assistant → Einstellungen → Geräte & Dienste → MQTT

3. **Topic-Prefix korrekt?**
   - Standard: `homeassistant/sensor/disk2iso`
   - Prüfe in `/opt/disk2iso/conf/libmqtt.ini`

### Logs prüfen

```bash
# Web-UI Logs
sudo journalctl -u disk2iso-web -f

# disk2iso Service Logs
sudo journalctl -u disk2iso -f

# MQTT Debug
tail -f /opt/disk2iso/logs/disk2iso.log | grep MQTT
```

## 📖 Dokumentation

- **Hauptdokumentation:** [disk2iso Handbuch](https://github.com/DirkGoetze/disk2iso/blob/master/doc/Handbuch.md)
- **MQTT-Modul Kapitel:** [04-5_MQTT.md](https://github.com/DirkGoetze/disk2iso/blob/master/doc/04_Module/04-5_MQTT.md)
- **Entwickler-Guide:** [06_Entwickler.md](https://github.com/DirkGoetze/disk2iso/blob/master/doc/06_Entwickler.md)

## 🤝 Contributing

Beiträge sind willkommen! Bitte beachte:

1. **Issues:** Bug-Reports und Feature-Requests via [GitHub Issues](https://github.com/DirkGoetze/disk2iso-mqtt/issues)
2. **Pull Requests:** Fork → Branch → Changes → PR
3. **Code-Style:** Folge dem [Modul-CLI-Interface-Pattern](https://github.com/DirkGoetze/disk2iso/blob/master/todo/Modul-CLI-Interface-Pattern.md)

## 📝 Changelog

Siehe [CHANGELOG.md](CHANGELOG.md) für Änderungen.

## 📄 Lizenz

MIT License - siehe [LICENSE](LICENSE)

## 🔗 Links

- **Haupt-Repository:** [disk2iso](https://github.com/DirkGoetze/disk2iso)
- **Issues:** [GitHub Issues](https://github.com/DirkGoetze/disk2iso-mqtt/issues)
- **Releases:** [GitHub Releases](https://github.com/DirkGoetze/disk2iso-mqtt/releases)

---

**Entwickelt mit ❤️ für die disk2iso Community**
