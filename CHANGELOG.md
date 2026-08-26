# Changelog

Alle relevanten Änderungen an diesem Blueprint werden hier dokumentiert.

## [1.1.1] - 2026-08-26

### Geändert

- README auf das feste Repository `vandecook/motion-light-plus` angepasst
- Platzhalter `<USER>` und `<REPO>` entfernt
- Hinweise zum Hochladen aus der README entfernt
- Blueprint-Beschreibung erweitert und Funktionsweise, Overrides, Prioritäten sowie Sicherheitslogik direkt im Blueprint dokumentiert
- `source_url` zum GitHub-Repository im Blueprint ergänzt
- Versionsangabe im Blueprint-Header mit README und Changelog synchronisiert

## [1.1.0] - 2026-08-26

### Geändert

- Lux-Grenzwert gilt nur noch im Tagesbetrieb
- Im konfigurierten Nachtzeitfenster wird die Lux-Prüfung übersprungen, damit Bewegung das gedimmte Nachtlicht zuverlässig einschalten kann
- Rückkehr aus Dauerlicht/Putzmodus berücksichtigt tagsüber ebenfalls den Lux-Grenzwert

## [1.0.0] - 2026-08-26

### Hinzugefügt

- Unterstützung für einen oder mehrere Bewegungs-/Präsenzsensoren
- Optionaler Helligkeitssensor mit Lux-Grenzwert
- Konfigurierbare Tageshelligkeit
- Optionaler Nachtmodus mit eigener Helligkeit und Zeitfenster
- Optionale Farbtemperatur für Tag, Nacht, Dauerlicht und Putzmodus
- Optionaler Dauerlicht-Override
- Optionaler Putzmodus über `input_button`
- Optionaler Schalter zum vollständigen Deaktivieren der Automatik
- Restart-Fallback nach Home-Assistant-Neustart
- Watchdog gegen dauerhaft eingeschaltetes Licht ohne Bewegung
