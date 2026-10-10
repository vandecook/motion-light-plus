# 💡 Motion Light Plus

A flexible but deliberately compact **Home Assistant motion-light blueprint** for rooms where a simple motion trigger is not quite enough.

**Version:** `1.2.0`  
**Repository:** `vandecook/motion-light-plus`

---

## ✨ Features

- 🚶 **One or multiple motion / presence sensors**
- ☀️ **Optional illuminance sensor** with daytime lux threshold
- 🌙 **Night mode** with dedicated brightness, color temperature and transitions
- 😴 **Sleep mode** as a sub-mode of night mode
- ⏱️ Configurable **maximum light-on duration in sleep mode**
- 🔒 Optional **continuous-light override**
- 🧹 Optional **cleaning mode** with temporary 100 % brightness
- ⛔ Optional **automation-off switch** for completely manual control
- 🌡️ Optional **color-temperature control**
- 🎚️ Separate **fade-in / fade-out** values for every operating mode
- 🔄 **Restart fallback** after a Home Assistant restart
- 🛡️ **Watchdog** against unintentionally persistent lighting

---

## 🇩🇪 Funktionsprinzip

### 🚶 Bewegung

Einer oder mehrere Bewegungs- bzw. Präsenzmelder können ausgewählt werden.

- Sobald **mindestens ein Sensor `on`** meldet, gilt Bewegung als erkannt.
- Keine Bewegung liegt erst vor, wenn **alle Sensoren `off`** sind.
- Ab diesem Zeitpunkt beginnt die konfigurierbare Nachlaufzeit.
- Vor dem Ausschalten wird erneut geprüft, ob weiterhin keine Bewegung vorliegt.

Damit eignet sich der Blueprint auch für Flure, Stiegenhäuser oder größere Räume mit mehreren Sensoren.

### ☀️ Tagesbetrieb und Lux-Sensor

Der Helligkeitssensor ist optional.

Ist er konfiguriert, wird das Licht im normalen Tagesbetrieb nur eingeschaltet, wenn der Messwert unterhalb des eingestellten Lux-Grenzwerts liegt. Ohne Lux-Sensor wird diese Prüfung vollständig übersprungen.

Die Lux-Prüfung gilt **nicht** für Nacht- und Schlafmodus.

### 🌙 Nachtmodus

Der Nachtmodus besitzt ein eigenes Zeitfenster und eigene Werte für:

- Helligkeit
- Farbtemperatur
- Fade-In
- Fade-Out

Zeitfenster über Mitternacht, beispielsweise `22:00 – 06:00`, werden unterstützt. Die Lux-Prüfung wird im Nachtmodus bewusst ignoriert.

**Beispiel:**

```text
Nachtmodus aktiv:       Ja
Beginn:                 22:00
Ende:                   06:00
Helligkeit:             20 %
Farbtemperatur:         2500 K
Fade-In:                1 s
Fade-Out:               2 s
```

Damit wird bei Bewegung zwischen 22:00 und 06:00 Uhr unabhängig vom Lux-Wert mit reduziertem, wärmerem Licht eingeschaltet.

### 😴 Schlafmodus

Der Schlafmodus ist bewusst als **Untermodus des Nachtmodus** ausgeführt.

Das Schlaf-Zeitfenster sollte vollständig innerhalb des Nacht-Zeitfensters liegen, beispielsweise:

```text
Nachtmodus:  22:00 – 06:00
Schlafmodus: 23:30 – 05:30
```

**Beispielkonfiguration Schlafmodus:**

```text
Schlafmodus aktiv:      Ja
Beginn:                 23:30
Ende:                   05:30
Helligkeit:             5 %
Farbtemperatur:         2200 K
Max. Leuchtdauer:       60 s
Fade-In:                2 s
Fade-Out:               3 s
```

Der Schlafmodus besitzt eigene Werte für:

- Helligkeit
- Farbtemperatur
- Fade-In
- Fade-Out
- maximale Leuchtdauer pro Aktivierung

Die maximale Leuchtdauer begrenzt eine Schlafmodus-Aktivierung auch dann, wenn weiterhin Bewegung erkannt wird. Eine spätere neue Bewegung kann das Licht erneut aktivieren.

### 🔒 Dauerlicht

Optional kann ein `switch` oder `input_boolean` als Dauerlicht-Override verwendet werden.

Solange dieser aktiv ist:

- bleibt das Licht eingeschaltet,
- die Lux-Prüfung wird ignoriert,
- Nachlaufzeit und Watchdog dürfen das Licht nicht ausschalten.

Für Dauerlicht stehen eigene Helligkeits-, Farbtemperatur- und Übergangswerte zur Verfügung.

### 🧹 Putzmodus

Optional kann ein `input_button` als Putz-Taster verwendet werden.

Ein Tastendruck:

1. schaltet das Licht auf **100 %**,
2. verwendet die konfigurierte Putz-Farbtemperatur,
3. hält den Modus für die eingestellte Dauer aktiv.

Ein erneuter Tastendruck startet die Putzzeit erneut.

### ⛔ Automatik aus

Ein optionaler `switch` oder `input_boolean` kann die komplette Blueprint-Steuerung sperren.

Ist dieser Schalter aktiv, verändert der Blueprint weder:

- Ein/Aus-Zustand,
- Helligkeit,
- noch Farbtemperatur.

Die Leuchte kann vollständig manuell bedient werden.

---

## 🥇 Prioritäten

Wenn mehrere Funktionen gleichzeitig zutreffen, gilt folgende Reihenfolge:

1. ⛔ **Automatik aus**
2. 🔒 **Dauerlicht**
3. 🧹 **Putzmodus**
4. 😴 **Schlafmodus**
5. 🌙 **Nachtmodus**
6. ☀️ **Normalbetrieb**
7. 🛡️ **Watchdog**

Schlaf- und Nachtmodus sind dabei zeitabhängige Untermodi der normalen Bewegungssteuerung und keine klassischen manuellen Overrides.

---

## 🎚️ Fade-In und Fade-Out

Jeder relevante Lichtmodus besitzt eigene Übergangszeiten:

| Modus | Fade-In | Fade-Out |
|---|---:|---:|
| ☀️ Normal | ✅ | ✅ |
| 🌙 Nacht | ✅ | ✅ |
| 😴 Schlaf | ✅ | ✅ |
| 🔒 Dauerlicht | ✅ | ✅ |
| 🧹 Putzmodus | ✅ | ✅ |

`0 s` bedeutet einen unmittelbaren Wechsel ohne Übergang.

Ob und wie sauber Übergänge ausgeführt werden, hängt von der verwendeten Leuchte bzw. Integration ab.

---

## 🌡️ Farbtemperatur

Die Farbtemperatursteuerung kann global deaktiviert werden. Das ist sinnvoll für reine On/Off- oder dimmbare Leuchten ohne Color-Temperature-Unterstützung.

Wenn aktiviert, können separate Kelvin-Werte für folgende Modi definiert werden:

- Normalbetrieb
- Nachtmodus
- Schlafmodus
- Dauerlicht
- Putzmodus

### 💡 Beispielwerte

Die folgenden Werte sind nur praxisnahe Startpunkte und können je nach Leuchte und Raum angepasst werden:

| Modus | Helligkeit | Farbtemperatur | Typischer Eindruck |
|---|---:|---:|---|
| ☀️ Normal | 80–100 % | 3500–4500 K | neutral / funktional |
| 🌙 Nacht | 10–30 % | 2400–2700 K | warm und zurückhaltend |
| 😴 Schlaf | 1–10 % | 2000–2300 K | sehr warm, möglichst wenig störend |
| 🔒 Dauerlicht | 80–100 % | 3500–4500 K | normaler Raumkomfort |
| 🧹 Putzmodus | 100 % | 4000–5000 K | hell und funktional |

Wenn **Farbtemperatur verwenden** deaktiviert ist, werden keine `color_temp_kelvin`-Werte an die Leuchte gesendet.

---

## 🔄 Verhalten nach Home-Assistant-Neustart

Laufende Verzögerungen können bei einem Neustart verloren gehen. Deshalb enthält der Blueprint einen zusätzlichen Start-Fallback.

Nach dem Start:

1. wartet der Blueprint eine konfigurierbare Initialisierungszeit,
2. berücksichtigt Dauerlicht und einen noch laufenden Putzmodus,
3. prüft bei eingeschaltetem Licht die Bewegungsmelder,
4. startet bei fehlender Bewegung eine neue Sicherheits-Nachlaufzeit.

Dadurch soll ein vor dem Neustart eingeschaltetes Licht nicht unbegrenzt eingeschaltet bleiben.

---

## 🛡️ Watchdog

Der Watchdog prüft periodisch, ob ein Licht ungewöhnlich lange eingeschaltet ist, obwohl keine Bewegung erkannt wird.

Er greift **nicht** ein, wenn:

- Automatik aus aktiv ist,
- Dauerlicht aktiv ist,
- oder der Putzmodus noch läuft.

---

## ⚙️ Benötigte Entitäten

### Pflicht

- mindestens ein `binary_sensor` für Bewegung oder Präsenz
- eine `light`-Entity

### Optional

- `sensor` mit `device_class: illuminance`
- `input_boolean` oder `switch` für Dauerlicht
- `input_button` für Putzmodus
- `input_boolean` oder `switch` für „Automatik aus“

---

## 🧩 Empfohlene Helper

Die optionalen Override-Funktionen benötigen keine speziellen Integrationen. Normale Home-Assistant-Helper reichen aus.

Für einen Flur könnten beispielsweise folgende Helper angelegt werden:

```text
input_boolean.flur_dauerlicht
input_boolean.flur_automatik_aus
input_button.flur_putzen
```

Bedeutung:

- 🔒 `input_boolean.flur_dauerlicht` – hält das Licht dauerhaft eingeschaltet.
- ⛔ `input_boolean.flur_automatik_aus` – der Blueprint greift überhaupt nicht mehr in das Licht ein.
- 🧹 `input_button.flur_putzen` – startet den zeitlich begrenzten Putzmodus.

Es ist **nicht notwendig, alle Helper anzulegen**. Nicht benötigte Funktionen können im Blueprint einfach leer bleiben.

---

## ⚙️ Gesamte Beispielkonfiguration

Beispiel für einen Flur mit zwei Bewegungsmeldern, Lux-Sensor, Nacht- und Schlafmodus:

```text
Geräte
  Bewegungsmelder:        binary_sensor.flur_eingang
                           binary_sensor.flur_gang
  Licht:                  light.flur
  Helligkeitssensor:      sensor.flur_illuminance

Normalbetrieb
  Lux-Grenzwert:          120 lx
  Helligkeit:             100 %
  Farbtemperatur:         4000 K
  Fade-In:                1 s
  Fade-Out:               2 s

Farbtemperatur
  Verwenden:              Ja

Nachtmodus
  Aktiv:                  Ja
  Zeit:                   22:00 – 06:00
  Helligkeit:             20 %
  Farbtemperatur:         2500 K
  Fade-In:                1 s
  Fade-Out:               2 s

Schlafmodus
  Aktiv:                  Ja
  Zeit:                   23:30 – 05:30
  Helligkeit:             5 %
  Farbtemperatur:         2200 K
  Max. Leuchtdauer:       60 s
  Fade-In:                2 s
  Fade-Out:               3 s

Bewegung & Sicherheit
  Nachlaufzeit:           180 s
  Restart-Wartezeit:      30 s
  Watchdog:               900 s

Dauerlicht
  Schalter:               input_boolean.flur_dauerlicht
  Helligkeit:             100 %
  Farbtemperatur:         4000 K
  Fade-In:                1 s
  Fade-Out:               2 s

Putzmodus
  Taster:                 input_button.flur_putzen
  Dauer:                  1800 s
  Farbtemperatur:         4500 K
  Fade-In:                1 s
  Fade-Out:               2 s

Automatik
  Deaktivieren:           input_boolean.flur_automatik_aus
```

### 🕒 Beispielhafter Tagesablauf

```text
07:00 – 21:59   Normalbetrieb
                → Lux-Sensor wird berücksichtigt

22:00 – 23:29   Nachtmodus
                → 20 %, 2500 K

23:30 – 05:29   Schlafmodus
                → 5 %, 2200 K, max. 60 s je Aktivierung

05:30 – 05:59   Nachtmodus
                → wieder 20 %, 2500 K

ab 06:00        Normalbetrieb
                → Lux-Sensor wird wieder berücksichtigt
```

---

## 📥 Import in Home Assistant

Repository:

```text
https://github.com/vandecook/motion-light-plus
```

Blueprint:

```text
https://github.com/vandecook/motion-light-plus/blob/main/blueprints/automation/motion_light_plus.yaml
```

Raw URL:

```text
https://raw.githubusercontent.com/vandecook/motion-light-plus/main/blueprints/automation/motion_light_plus.yaml
```

Nach dem Import wird aus dem Blueprint eine normale Home-Assistant-Automation erstellt. Für jede Instanz kann dabei ein eigener Name bzw. Alias vergeben werden, beispielsweise:

- `Flur Bewegungslicht`
- `WC Bewegungslicht`
- `Keller Bewegungslicht`

---

## 🇬🇧 English summary

Motion Light Plus controls a light using one or more motion/presence sensors. An optional lux sensor can suppress switching during daytime. Night and sleep modes provide dedicated brightness, color temperature and transitions. Sleep mode can additionally limit the maximum light-on duration per activation.

Optional manual modes include continuous light, cleaning mode and a complete automation lockout. A restart fallback and watchdog provide additional protection against a light remaining on unintentionally.

Priority:

```text
Automation off
→ Continuous light
→ Cleaning mode
→ Sleep mode
→ Night mode
→ Normal motion mode
→ Watchdog
```

---

## 📄 License

Use and modification are permitted according to the license provided with the repository.
