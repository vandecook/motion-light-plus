# Changelog

Alle relevanten Änderungen an diesem Blueprint werden hier dokumentiert.

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
