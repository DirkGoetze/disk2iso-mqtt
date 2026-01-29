# Changelog

Alle wichtigen Änderungen am MQTT-Modul werden hier dokumentiert.

Das Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.0.0/),
und dieses Projekt folgt [Semantic Versioning](https://semver.org/lang/de/).

## [Unreleased]

## [1.0.0] - 2026-01-29

### Added
- 🎉 Initiales Release des MQTT-Moduls
- Observer Pattern für automatische Status-Updates
- CLI-Interface für Scripting (export-config, update-config, test-connection)
- Helper-Funktionen für Code-Wiederverwendung
- Single Source of Truth für Default-Werte
- Web-UI Widget für Index-Seite (Service-Status)
- Web-UI Config-Widget mit Auto-Save Funktion
- Blueprint-System für modulare Flask-Routen
- Home Assistant Auto-Discovery Support
- Echtzeit-Status-Updates (idle, copying, waiting, completed, error)
- Fortschritts-Tracking (Prozent, MB, ETA)
- Medium-Informationen (Label, Typ, Größe)
- Availability-Monitoring (online/offline)
- Internationalisierung (de, en, es, fr)

### Changed
- Python nutzt jetzt CLI-Interface statt direkter Config-Zugriffe
- MQTT-Config aus Haupt-App ausgelagert in Blueprint
- Compliance: 55% → 100% (Zero Business-Logic in Python)

### Technical
- Observer Pattern: mqtt_publish_from_api() triggert via API-Änderungen
- Three-Flag Pattern: SUPPORT_MQTT, INITIALIZED_MQTT, ACTIVATED_MQTT
- Code-Reduktion: -35 Zeilen durch DRY-Prinzip
- 100% Nutzung von libconfig.sh (get_ini_value + Setter)

[Unreleased]: https://github.com/DirkGoetze/disk2iso-mqtt/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/DirkGoetze/disk2iso-mqtt/releases/tag/v1.0.0