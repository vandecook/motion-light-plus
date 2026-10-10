# 💡 Motion Light Plus

A flexible but deliberately compact **Home Assistant motion-light blueprint** for rooms where a simple motion trigger is not quite enough.

**Version:** `1.2.1`  
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

Zeitfenster über Mitternacht, beispielsweise `22:00 – 06:00`, werden unterstützt.

### 😴 Schlafmodus

Der Schlafmodus ist bewusst als **Untermodus des Nachtmodus** ausgeführt.

Das Schlaf-Zeitfenster sollte vollständig innerhalb des Nacht-Zeitfensters liegen, beispielsweise:

```text
Nachtmodus:  22:00 – 06:00
Schlafmodus: 23:30 – 05:30
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

### 💡 Orientierung für Kelvin-Werte

| Farbtemperatur | Wirkung | Typische Verwendung |
|---:|---|---|
| **2000–2200 K** | sehr warm, amberartig | Schlafmodus, nächtliche Orientierung |
| **2400–2700 K** | warmweiß | Nachtmodus, gemütliche Beleuchtung |
| **3000 K** | warmweiß bis neutral | Wohnräume |
| **3500–4000 K** | neutralweiß | Normalbetrieb, Flur, Küche |
| **4500–5000 K** | eher kühl und klar | Putzmodus, Arbeitslicht |
| **5500–6500 K** | tageslichtweiß | sehr helles Funktionslicht |

### 🏠 Beispielkonfiguration

Eine mögliche Abstimmung für einen Flur wäre:

| Modus | Helligkeit | Farbtemperatur |
|---|---:|---:|
| ☀️ Normalbetrieb | 100 % | 4000 K |
| 🌙 Nachtmodus | 20 % | 2500 K |
| 😴 Schlafmodus | 5 % | 2200 K |
| 🔒 Dauerlicht | 100 % | 4000 K |
| 🧹 Putzmodus | 100 % | 4500 K |

Die Werte sind nur Ausgangspunkte. Je nach Leuchte unterscheiden sich wahrgenommene Helligkeit und Farbtemperatur deutlich.

> **Hinweis:** Manche Leuchten unterstützen nur einen eingeschränkten Kelvin-Bereich. In diesem Fall müssen die Werte innerhalb des von der jeweiligen `light`-Entity unterstützten Bereichs gewählt werden.

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

## ⚡ Ereignis- und Performance-Verhalten

Optionale Eingänge wie Dauerlicht und Putz-Taster werden **gezielt nach Entity-ID gefiltert**. Der Blueprint lauscht nicht global auf alle `state_changed`-Events von Home Assistant.

Dadurch erzeugen häufig aktualisierte Sensoren, Leistungswerte, MQTT-Entitäten oder andere Zustandsänderungen keine unnötigen Ausführungen dieser Automation.

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
