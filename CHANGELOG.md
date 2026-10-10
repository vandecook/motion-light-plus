# Changelog

Alle relevanten Änderungen an **Motion Light Plus** werden hier dokumentiert.

Das Projekt verwendet [Semantic Versioning](https://semver.org/).

## [1.2.0] - 2026-10-10

### Hinzugefügt

- Neuer optionaler **Schlafmodus** als Untermodus des Nachtmodus
- Eigenes Schlafmodus-Zeitfenster innerhalb des Nachtmodus
- Eigene Helligkeit für den Schlafmodus
- Eigene Farbtemperatur für den Schlafmodus
- Eigene Fade-In- und Fade-Out-Zeiten für den Schlafmodus
- Konfigurierbare maximale Leuchtdauer pro Aktivierung im Schlafmodus
- Erneute Aktivierung des Schlafmodus durch eine neue Bewegung nach Ablauf der maximalen Leuchtdauer
- Konfigurierbare **Fade-In- und Fade-Out-Zeiten für alle relevanten Betriebsmodi**:
  - Normalbetrieb
  - Nachtmodus
  - Schlafmodus
  - Dauerlicht
  - Putzmodus
- Erweiterte README mit:
  - empfohlenen Farbtemperaturen
  - empfohlenen Home-Assistant-Helpern
  - vollständiger Beispielkonfiguration
  - Nachtmodus-Beispiel
  - Schlafmodus-Beispiel
  - beispielhaftem Tagesablauf
- Separate `REQUIREMENTS.md` mit nummerierten Anforderungen

### Geändert

- Betriebspriorität um Schlafmodus erweitert:
  1. Automatik aus
  2. Dauerlicht
  3. Putzmodus
  4. Schlafmodus
  5. Nachtmodus
  6. Normalbetrieb
  7. Watchdog
- Nacht- und Schlafmodus werden als zeitabhängige Untermodi der Bewegungsautomatik behandelt
- Lux-Prüfung bleibt auf den normalen Tagesbetrieb beschränkt
- README an den erweiterten Funktionsumfang angepasst

## [1.1.2] - 2026-08-26

### Geändert

- Blueprint-Beschreibung visuell überarbeitet und klarer strukturiert
- Vollständige Kurzbeschreibung in Deutsch und Englisch ergänzt
- Grundfunktion, optionale Betriebsarten, Sicherheitslogik und Prioritäten direkt in der Blueprint-Beschreibung verständlicher dargestellt

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
