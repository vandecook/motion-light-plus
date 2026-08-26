# 💡 Motion Light Plus – Home Assistant Blueprint

Ein Home-Assistant-Automation-Blueprint für robuste Bewegungs-/Präsenzlichtsteuerung mit mehreren Sensoren, optionalem Lux-Sensor, Nachtbetrieb, optionaler Farbtemperatur, Overrides, Putzmodus, Restart-Fallback und Watchdog.

🔗 **Repository:** `vandecook/motion-light-plus`


## 📦 GitHub-Struktur

```text
motion-light-plus/
├── blueprints/
│   └── automation/
│       └── motion_light_plus.yaml
├── CHANGELOG.md
├── LICENSE
└── README.md
```

Der Blueprint kann in Home Assistant direkt aus diesem GitHub-Repository importiert werden.

GitHub-Datei-URL:

```text
https://github.com/vandecook/motion-light-plus/blob/main/blueprints/automation/motion_light_plus.yaml
```

Alternativ funktioniert auch die Raw-URL:

```text
https://raw.githubusercontent.com/vandecook/motion-light-plus/main/blueprints/automation/motion_light_plus.yaml
```



## 🏷️ Version

Aktuelle Version: **v1.1.2**

Die Versionsnummer folgt [Semantic Versioning](https://semver.org/):

- **MAJOR** – inkompatible Änderungen
- **MINOR** – neue, rückwärtskompatible Funktionen
- **PATCH** – Fehlerbehebungen und kleine Korrekturen

Für GitHub-Releases empfiehlt sich derselbe Tag, z. B. `v1.1.2`.

## ✨ Funktionsumfang

- Ein oder mehrere Bewegungs-/Präsenzsensoren
- Licht an, sobald mindestens ein ausgewählter Sensor Bewegung meldet
- Ausschalten erst, wenn nach dem letzten `off`-Ereignis die Nachlaufzeit abgelaufen ist **und alle** ausgewählten Sensoren weiterhin `off` sind
- Optionaler Helligkeitssensor mit **Tages-Lux-Grenzwert**
- Konfigurierbare Tageshelligkeit
- Optionaler Nachtmodus mit eigener Helligkeit und Zeitfenster, auch über Mitternacht
- Optional konfigurierbare Farbtemperatur in Kelvin für Tag, Nacht, Dauerlicht und Putzmodus
- Optionaler Dauerlicht-Override
- Optionaler Putzmodus über `input_button`, standardmäßig 100 % für eine definierte Zeit
- Optionaler Schalter „Automatik deaktivieren“
- Restart-Fallback nach Home-Assistant-Neustart
- Watchdog gegen dauerhaft eingeschaltetes Licht ohne Bewegung

## 🧭 Prioritäten

Die Logik arbeitet in folgender Priorität:

1. **Automatik deaktiviert** – der Blueprint fasst das Licht überhaupt nicht an.
2. **Dauerlicht** – das Licht wird aktiv eingeschaltet und nicht automatisch ausgeschaltet.
3. **Putzmodus** – das Licht läuft zeitlich begrenzt auf 100 %.
4. **Bewegungsautomatik** – normale Lux-/Tag-/Nacht-Logik.
5. **Watchdog** – Sicherheitsabschaltung, falls das Licht ohne Bewegung hängen bleibt.

## 🚶 Bewegungsmelder

Im Blueprint können direkt mehrere `binary_sensor`-Entitäten ausgewählt werden. Eine bestehende Home-Assistant-Gruppe kann ebenfalls verwendet werden, sofern sie als geeigneter `binary_sensor` vorliegt.

Die Ausschaltlogik verwendet bewusst keinen einfachen `for:`-Trigger pro Sensor. Sobald ein Sensor auf `off` wechselt, startet die Nachlaufzeit. Nach Ablauf wird erneut geprüft, ob **alle** gewählten Bewegungssensoren `off` sind. Neue Bewegung während der Nachlaufzeit verhindert somit das Ausschalten.

## ☀️ Helligkeitssensor

Der Lux-Sensor ist optional und wird **nur im Tagesbetrieb** ausgewertet.

- Kein Sensor ausgewählt: Lux-Prüfung wird übersprungen.
- Tagsüber: Das Licht wird bei Bewegung nur eingeschaltet, wenn der Wert unter dem eingestellten Tages-Lux-Grenzwert liegt.
- Nachtmodus aktiv: Die Lux-Prüfung wird bewusst übersprungen; Bewegung kann das gedimmte Nachtlicht unabhängig vom Lux-Wert einschalten.
- Sensor liefert tagsüber `unknown` oder `unavailable`: Das Licht wird durch Bewegung nicht eingeschaltet, bis wieder ein gültiger Messwert vorliegt.

Beispiel: Bei `150 lx` als Grenzwert bleibt das Licht tagsüber ab 150 lx aus. Im definierten Nachtzeitfenster greift stattdessen die Nacht-Helligkeit.

## 🌙 Nachtmodus

Beispiel:

- Beginn: `22:00`
- Ende: `06:00`
- Helligkeit: `20 %`
- Farbtemperatur: `2500 K`

Zeitfenster über Mitternacht werden unterstützt.

## 🎨 Farbtemperatur

Die Farbtemperatur ist global optional. Aktiviere **Use color temperature** nur für Leuchten, die einstellbares Weiß unterstützen.

Typische Startwerte:

| Modus | Helligkeit | Farbtemperatur |
|---|---:|---:|
| Tag | 100 % | 4000 K |
| Nacht | 20 % | 2500 K |
| Dauerlicht | 100 % | 4000 K |
| Putzmodus | 100 % | 4500 K |

Wenn Farbtemperatur deaktiviert ist, werden keine `color_temp_kelvin`-Parameter an `light.turn_on` übergeben.

## 🔒 Dauerlicht-Override

Optional kann ein `input_boolean` oder `switch` gewählt werden. Ist dieser `on`, wird das Licht mit der konfigurierten Override-Helligkeit eingeschaltet. Automatische Ausschaltung und Watchdog werden unterdrückt.

Der Dauerlicht-Override ist **nicht** dasselbe wie „Automatik deaktivieren“: Dauerlicht steuert das Licht aktiv; Automatik deaktivieren lässt das Licht vollständig in Ruhe.

## 🧹 Putzmodus

Optional kann ein `input_button` ausgewählt werden.

Beim Drücken:

1. Licht sofort auf 100 %.
2. Optional eigene Farbtemperatur.
3. Nach der konfigurierten Zeit Rückkehr zur normalen Logik.
4. Ein erneuter Tastendruck verlängert den Modus ab diesem Tastendruck.

Der Zustand eines `input_button` enthält den Zeitpunkt der letzten Betätigung. Dadurch kann der Blueprint nach einem Home-Assistant-Neustart prüfen, ob die konfigurierte Putzzeit noch aktiv ist.

## ⏸️ Automatik deaktivieren

Optional kann ein `input_boolean` oder `switch` gewählt werden. Solange er `on` ist, beendet der Blueprint jede ausgelöste Ausführung sofort und verändert weder Zustand noch Helligkeit oder Farbe des Lichts.

Beim späteren Ausschalten dieses Schalters wird kein Lichtzustand erzwungen. Die Automatik reagiert wieder auf die nächsten normalen Ereignisse.

## 🔄 Restart-Fallback

Laufende Verzögerungen einer Automation sind nicht als persistenter Timer zu betrachten. Deshalb prüft der Blueprint beim Start von Home Assistant nach einer konfigurierbaren Initialisierungszeit:

- Automatik deaktiviert → nichts tun
- Dauerlicht aktiv → Dauerlicht wieder anwenden
- Putzmodus noch aktiv → Putzmodus anhand des letzten Button-Zeitpunkts wiederherstellen
- Licht an + keine Bewegung → neue Sicherheits-Nachlaufzeit starten und danach erneut prüfen

Dadurch bleibt ein Licht nach einem Neustart nicht unbegrenzt eingeschaltet, nur weil eine vorher laufende Ausschaltverzögerung verloren gegangen ist.

## 🛡️ Watchdog

Der Watchdog prüft alle fünf Minuten. Er schaltet das Licht nur aus, wenn:

- das Licht `on` ist,
- alle Bewegungssensoren `off` sind,
- Dauerlicht nicht aktiv ist,
- kein Putzmodus aktiv ist,
- „Automatik deaktivieren“ nicht aktiv ist,
- und das Licht länger als die Watchdog-Zeit eingeschaltet ist.

**Hinweis:** Damit kann auch ein manuell eingeschaltetes Licht automatisch ausgeschaltet werden. Wenn manuell gesteuertes Licht unangetastet bleiben soll, vorher „Automatik deaktivieren“ verwenden.

## 📦 Installation

Lege die Blueprint-Datei z. B. hier ab:

```text
/config/blueprints/automation/<dein_name>/motion_light_plus.yaml
```

Danach in Home Assistant:

1. **Einstellungen → Automatisierungen & Szenen → Blueprints** öffnen.
2. Blueprint neu laden bzw. Home Assistant die Datei erkennen lassen.
3. **Automation erstellen** wählen.
4. Mindestens Bewegungsmelder und Licht konfigurieren.
5. Optionale Funktionen nur dann belegen, wenn sie benötigt werden.

## 🧩 Empfohlene Helper

Für alle optionalen Funktionen sind normale Home-Assistant-Helper ausreichend:

```text
input_boolean.flur_dauerlicht
input_boolean.flur_automatik_aus
input_button.flur_putzen
```

Es ist nicht notwendig, alle drei anzulegen.

## ⚙️ Beispielkonfiguration

Für einen Flur könnte eine Instanz etwa so eingestellt werden:

```text
Bewegungsmelder:        PIR Eingang + PIR Gang
Licht:                  light.flur
Lux-Sensor:             sensor.flur_illuminance
Tages-Lux-Grenze:      80 lx
Tag-Helligkeit:         100 %
Nachtzeit:              22:00–06:00
Nacht-Helligkeit:       20 %
Farbtemperatur:         aktiviert
Tag:                     4000 K
Nacht:                   2500 K
Nachlaufzeit:           180 s
Restart-Wartezeit:      30 s
Watchdog:               900 s
Dauerlicht:             input_boolean.flur_dauerlicht
Putzmodus:              input_button.flur_putzen
Putzdauer:              1800 s
Automatik deaktivieren: input_boolean.flur_automatik_aus
```

## 🔧 Technische Hinweise

Der Blueprint verwendet Input Sections und benötigt deshalb Home Assistant `2024.6.0` oder neuer.

`light.turn_on` verwendet `brightness_pct` und optional `color_temp_kelvin`. Nicht jede Leuchte unterstützt Farbtemperatur; in diesem Fall die Farbtemperatur-Funktion deaktiviert lassen.

Die Automation läuft mit `mode: parallel`. Das ist absichtlich so gewählt: Mehrere Sensorereignisse und Sicherheitsprüfungen dürfen gleichzeitig laufen. Vor jeder automatischen Abschaltung werden Bewegung und Overrides erneut geprüft.

## 📁 Dateien

- `motion_light_plus.yaml` – Blueprint
- `README.md` – Dokumentation
