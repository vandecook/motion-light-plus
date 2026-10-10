# Motion Light Plus – Requirements

**Dokumenttyp:** Requirements Specification  
**Projekt:** Home Assistant Blueprint – Motion Light Plus  
**Stand:** 2026-10-10  
**Version:** 1.0

---

## 1. Allgemeine Anforderungen

### REQ-001 – Blueprint-basierte Umsetzung
Die Lichtsteuerung muss als Home-Assistant-Automation-Blueprint umgesetzt werden.

### REQ-002 – Mehrfachverwendung
Der Blueprint muss mehrfach für unterschiedliche Räume bzw. Lichtkreise instanziierbar sein.

### REQ-003 – Individueller Alias
Für jede aus dem Blueprint erzeugte Automation muss in Home Assistant ein individueller Name bzw. Alias vergeben werden können.

### REQ-004 – Einfache Konfiguration
Die Konfiguration muss auf die wesentlichen Parameter beschränkt bleiben. Optionale Funktionen dürfen bei Nichtverwendung keinen unnötigen Konfigurationsaufwand verursachen.

### REQ-005 – Optionale Funktionen
Optionale Funktionen müssen deaktiviert bzw. ohne zugeordnete Entity betrieben werden können, ohne die Grundfunktion des Blueprints zu beeinträchtigen.

---

## 2. Bewegungs- und Präsenzmelder

### REQ-010 – Mehrere Bewegungsmelder
Es müssen ein oder mehrere Bewegungs- bzw. Präsenzmelder ausgewählt werden können.

### REQ-011 – Einschalten bei Bewegung
Das Licht muss eingeschaltet werden, sobald mindestens einer der konfigurierten Bewegungsmelder Bewegung erkennt.

### REQ-012 – Ausschalten erst bei keiner Bewegung
Das Licht darf erst ausgeschaltet werden, wenn keiner der konfigurierten Bewegungsmelder mehr Bewegung erkennt.

### REQ-013 – Konfigurierbare Nachlaufzeit
Nach dem Wechsel auf „keine Bewegung“ muss eine konfigurierbare Nachlaufzeit gestartet werden.

### REQ-014 – Erneute Prüfung vor Ausschalten
Nach Ablauf der Nachlaufzeit muss erneut geprüft werden, ob weiterhin keine Bewegung erkannt wird.

### REQ-015 – Neue Bewegung während Nachlaufzeit
Wird während der Nachlaufzeit erneut Bewegung erkannt, darf das Licht nicht ausgeschaltet werden.

### REQ-016 – Sensor-Aktivzeit nicht künstlich duplizieren
Die interne Aktivzeit eines Bewegungsmelders muss nicht zusätzlich im Blueprint nachgebildet werden. Maßgeblich für die Nachlaufzeit ist der Zustandswechsel des Melders auf „keine Bewegung“.

---

## 3. Helligkeitssensor

### REQ-020 – Optionaler Helligkeitssensor
Die Verwendung eines Helligkeitssensors muss optional sein.

### REQ-021 – Lux-Grenzwert
Bei konfiguriertem Helligkeitssensor muss ein Grenzwert in Lux eingestellt werden können.

### REQ-022 – Einschaltunterdrückung bei ausreichender Helligkeit
Im Tagesbetrieb darf das Licht bei Bewegung nicht eingeschaltet werden, wenn die gemessene Raumhelligkeit oberhalb des konfigurierten Grenzwerts liegt.

### REQ-023 – Verhalten ohne Helligkeitssensor
Ist kein Helligkeitssensor konfiguriert, muss die Lux-Prüfung vollständig übersprungen werden.

### REQ-024 – Nicht verfügbarer Helligkeitssensor
Ist ein konfigurierter Helligkeitssensor `unknown` oder `unavailable`, darf dessen Messwert nicht als gültige Lux-Information verwendet werden.

### REQ-025 – Lux-Prüfung nur im Tagesbetrieb
Die Lux-Prüfung muss ausschließlich im normalen Tagesbetrieb angewendet werden. Nacht- und Schlafmodus dürfen davon nicht blockiert werden.

---

## 4. Normalbetrieb

### REQ-030 – Normale Helligkeit
Für den normalen Bewegungsbetrieb muss eine Zielhelligkeit in Prozent konfigurierbar sein.

### REQ-031 – Normale Farbtemperatur
Optional muss für den normalen Betrieb eine Farbtemperatur in Kelvin eingestellt werden können.

### REQ-032 – Optionale Farbtemperatursteuerung
Die Farbtemperatursteuerung muss deaktivierbar sein, damit auch Leuchten ohne Farbtemperaturunterstützung verwendet werden können.

### REQ-033 – Fade-In Normalbetrieb
Für das Einschalten im Normalbetrieb muss eine konfigurierbare Fade-In-Zeit unterstützt werden.

### REQ-034 – Fade-Out Normalbetrieb
Für das Ausschalten im Normalbetrieb muss eine konfigurierbare Fade-Out-Zeit unterstützt werden.

---

## 5. Nachtmodus

### REQ-040 – Optionaler Nachtmodus
Ein Nachtmodus muss optional aktivierbar sein.

### REQ-041 – Nachtzeitfenster
Für den Nachtmodus müssen Start- und Endzeit konfigurierbar sein.

### REQ-042 – Zeitfenster über Mitternacht
Das Nachtzeitfenster muss über Mitternacht hinweg funktionieren, beispielsweise 22:00 bis 06:00 Uhr.

### REQ-043 – Nacht-Helligkeit
Für den Nachtmodus muss eine separate Helligkeit konfigurierbar sein.

### REQ-044 – Nacht-Farbtemperatur
Für den Nachtmodus muss optional eine eigene Farbtemperatur konfigurierbar sein.

### REQ-045 – Lux-Prüfung im Nachtmodus
Im Nachtmodus muss die Lux-Prüfung ignoriert werden, sodass Bewegung das Licht unabhängig vom aktuellen Lux-Wert einschalten kann.

### REQ-046 – Fade-In Nachtmodus
Für das Einschalten im Nachtmodus muss eine eigene Fade-In-Zeit konfigurierbar sein.

### REQ-047 – Fade-Out Nachtmodus
Für das Ausschalten im Nachtmodus muss eine eigene Fade-Out-Zeit konfigurierbar sein.

---

## 6. Schlafmodus

Der Schlafmodus ist eine Unterfunktion des Nachtmodus und darf nur innerhalb des Nachtmodus aktiv werden.

### REQ-050 – Optionaler Schlafmodus
Innerhalb des Nachtbetriebs muss optional ein separater Schlafmodus aktivierbar sein.

### REQ-051 – Schlafmodus aktivieren/deaktivieren
Der Schlafmodus muss unabhängig aktiviert bzw. deaktiviert werden können.

### REQ-052 – Schlafmodus-Zeitfenster
Für den Schlafmodus müssen Start- und Endzeit konfigurierbar sein.

### REQ-053 – Schlafmodus über Mitternacht
Das Schlafmodus-Zeitfenster muss über Mitternacht hinweg funktionieren.

### REQ-054 – Schlafmodus innerhalb des Nachtmodus
Das Zeitfenster des Schlafmodus muss vollständig innerhalb des konfigurierten Nachtmodus-Zeitfensters liegen. Außerhalb des Nachtmodus darf der Schlafmodus nicht aktiv werden.

### REQ-055 – Fallback auf Nachtmodus
Liegt die aktuelle Uhrzeit innerhalb des Nachtmodus, aber außerhalb des Schlafmodus, muss automatisch die normale Nachtmodus-Konfiguration verwendet werden.

### REQ-056 – Schlafmodus-Helligkeit
Für den Schlafmodus muss eine eigene Zielhelligkeit konfigurierbar sein.

### REQ-057 – Schlafmodus-Farbtemperatur
Für den Schlafmodus muss eine eigene Farbtemperatur konfigurierbar sein.

### REQ-058 – Fade-In Schlafmodus
Beim Einschalten im Schlafmodus muss eine eigene Fade-In-Zeit konfigurierbar sein.

### REQ-059 – Fade-Out Schlafmodus
Beim Ausschalten im Schlafmodus muss eine eigene Fade-Out-Zeit konfigurierbar sein.

### REQ-060 – Maximale Leuchtdauer im Schlafmodus
Für den Schlafmodus muss eine maximale Leuchtdauer pro Aktivierung konfigurierbar sein.

### REQ-061 – Start der Schlafmodus-Dauer
Die maximale Leuchtdauer beginnt mit dem Einschalten des Lichts durch eine Bewegungserkennung im Schlafmodus.

### REQ-062 – Vorrang gegenüber allgemeiner Nachlaufzeit
Im Schlafmodus muss die maximale Leuchtdauer Vorrang vor der normalen Nachlaufzeit haben.

### REQ-063 – Bewegung verlängert nicht unbegrenzt
Weitere erkannte Bewegungen dürfen die maximale Leuchtdauer nicht unbegrenzt verlängern. Nach Ablauf der konfigurierten Schlafmodus-Dauer muss das Licht ausgeschaltet werden, sofern kein höher priorisierter Modus aktiv ist.

### REQ-064 – Erneute Aktivierung
Nach dem Ausschalten im Schlafmodus darf eine neue Bewegung das Licht erneut für die konfigurierte Schlafmodus-Dauer einschalten.

---

## 7. Dauerlicht-Override

### REQ-070 – Optionaler Dauerlicht-Schalter
Es muss optional ein Schalter bzw. `input_boolean` für Dauerlicht konfigurierbar sein.

### REQ-071 – Dauerlicht aktiv
Bei aktivem Dauerlicht-Override muss das Licht eingeschaltet bleiben.

### REQ-072 – Ausschaltlogik deaktivieren
Bei aktivem Dauerlicht-Override dürfen Bewegungs-Nachlauf, Restart-Fallback und Watchdog das Licht nicht ausschalten.

### REQ-073 – Lux-Prüfung deaktivieren
Der Lux-Grenzwert darf bei aktivem Dauerlicht-Override keinen Einfluss haben.

### REQ-074 – Override-Helligkeit
Für den Dauerlicht-Modus muss eine eigene Helligkeit konfigurierbar sein.

### REQ-075 – Override-Farbtemperatur
Optional muss für den Dauerlicht-Modus eine eigene Farbtemperatur konfigurierbar sein.

### REQ-076 – Fade-In Dauerlicht
Für den Dauerlicht-Modus muss eine konfigurierbare Fade-In-Zeit unterstützt werden.

### REQ-077 – Fade-Out Dauerlicht
Beim Verlassen des Dauerlicht-Modus muss eine konfigurierbare Fade-Out- bzw. Übergangszeit unterstützt werden.

---

## 8. Putzmodus

### REQ-080 – Optionaler Putz-Taster
Ein Putzmodus muss optional über einen `input_button` ausgelöst werden können.

### REQ-081 – Maximale Helligkeit
Beim Aktivieren des Putzmodus muss das Licht auf 100 % Helligkeit geschaltet werden.

### REQ-082 – Putzmodus-Dauer
Die Dauer des Putzmodus muss konfigurierbar sein.

### REQ-083 – Verlängerung durch erneuten Tastendruck
Ein erneuter Tastendruck muss die Laufzeit des Putzmodus ab diesem Zeitpunkt neu starten.

### REQ-084 – Putzmodus-Farbtemperatur
Optional muss für den Putzmodus eine eigene Farbtemperatur konfigurierbar sein.

### REQ-085 – Fade-In Putzmodus
Für den Putzmodus muss eine konfigurierbare Fade-In-Zeit unterstützt werden.

### REQ-086 – Fade-Out Putzmodus
Beim Ende des Putzmodus muss eine konfigurierbare Fade-Out- bzw. Übergangszeit unterstützt werden.

### REQ-087 – Ablauf des Putzmodus
Nach Ablauf des Putzmodus muss wieder anhand der aktuell gültigen Betriebsart entschieden werden, ob das Licht eingeschaltet bleibt oder ausgeschaltet wird.

---

## 9. Automatik deaktivieren

### REQ-090 – Optionaler Automatik-Aus-Schalter
Es muss optional ein Schalter bzw. `input_boolean` zum vollständigen Deaktivieren der Automatik vorgesehen werden.

### REQ-091 – Keine Lichtsteuerung bei deaktivierter Automatik
Bei aktivem Automatik-Aus-Schalter darf der Blueprint den Zustand, die Helligkeit oder die Farbtemperatur des Lichts nicht verändern.

### REQ-092 – Manuelle Bedienung
Während deaktivierter Automatik muss das Licht vollständig manuell bedienbar bleiben.

### REQ-093 – Wiederaktivierung
Nach dem Reaktivieren der Automatik darf keine unmittelbare Zustandsänderung erzwungen werden. Die Steuerung soll ab dem nächsten relevanten Ereignis wieder übernehmen.

---

## 10. Restart-Fallback

### REQ-100 – Home-Assistant-Neustart erkennen
Der Blueprint muss auf einen Neustart von Home Assistant reagieren.

### REQ-101 – Startverzögerung
Nach einem Neustart muss eine konfigurierbare Wartezeit vorgesehen werden, damit Sensoren und Integrationen ihre Zustände wiederherstellen können.

### REQ-102 – Hängendes Licht erkennen
Ist das Licht nach dem Neustart eingeschaltet und es liegt keine Bewegung vor, muss eine neue Sicherheits-Nachlaufzeit gestartet werden.

### REQ-103 – Zustände erneut prüfen
Vor dem Ausschalten nach einem Neustart müssen Bewegung, Dauerlicht, Putzmodus und Automatik-Aus erneut geprüft werden.

### REQ-104 – Putzmodus rekonstruieren
Ein vor dem Neustart gestarteter Putzmodus muss anhand des letzten Zeitstempels des Putz-Tasters soweit möglich rekonstruiert werden.

---

## 11. Watchdog

### REQ-110 – Watchdog-Funktion
Der Blueprint muss eine Sicherheitsprüfung gegen dauerhaft eingeschaltetes Licht enthalten.

### REQ-111 – Konfigurierbare Watchdog-Zeit
Die maximale zulässige Einschaltdauer ohne Bewegung muss konfigurierbar sein.

### REQ-112 – Watchdog-Ausschalten
Ist das Licht länger als die konfigurierte Watchdog-Zeit eingeschaltet und wird keine Bewegung erkannt, darf es ausgeschaltet werden.

### REQ-113 – Override-Berücksichtigung
Der Watchdog darf nicht eingreifen, wenn Automatik-Aus, Dauerlicht oder Putzmodus aktiv sind.

---

## 12. Fade- und Übergangsverhalten

### REQ-120 – Fade-Unterstützung für alle Lichtmodi
Normalbetrieb, Nachtmodus, Schlafmodus, Dauerlicht und Putzmodus müssen jeweils konfigurierbare Fade-In- und Fade-Out-Zeiten unterstützen.

### REQ-121 – Sofortiger Übergang
Ein Fade-Wert von `0 Sekunden` muss einen unmittelbaren Zustandswechsel ohne Übergang ermöglichen.

### REQ-122 – Modusspezifische Übergänge
Fade-In- und Fade-Out-Werte müssen je Betriebsmodus unabhängig voneinander konfigurierbar sein.

### REQ-123 – Übergang zwischen Modi
Beim Wechsel zwischen zwei aktiven Modi muss die für den Zielmodus vorgesehene Übergangszeit angewendet werden, sofern die verwendete Leuchte dies unterstützt.

---

## 13. Prioritäten

### REQ-130 – Betriebsartenpriorität
Die Betriebsarten müssen folgende Priorität besitzen:

1. Automatik aus
2. Dauerlicht
3. Putzmodus
4. Schlafmodus
5. Nachtmodus
6. Normaler Bewegungsbetrieb
7. Watchdog

### REQ-131 – Höhere Priorität gewinnt
Eine Betriebsart niedrigerer Priorität darf eine aktive Betriebsart höherer Priorität nicht überschreiben.

### REQ-132 – Schlaf- und Nachtmodus als Untermodi
Schlafmodus und Nachtmodus sind keine klassischen Overrides, sondern zeitabhängige Untermodi der Bewegungsautomatik.

---

## 14. Robustheit

### REQ-140 – Wiederholte Zustandsprüfung
Vor jeder verzögerten Ausschaltaktion müssen die für das Ausschalten relevanten Zustände nochmals geprüft werden.

### REQ-141 – Keine unerwartete Abschaltung
Eine während einer laufenden Verzögerung aktivierte höhere Betriebsart, beispielsweise Dauerlicht oder Automatik-Aus, muss eine spätere Ausschaltung verhindern.

### REQ-142 – Fehlerhafte Sensorzustände
`unknown`- oder `unavailable`-Zustände dürfen nicht ungeprüft als gültige Bewegungs- oder Helligkeitsinformation behandelt werden.

### REQ-143 – Verhalten bei nicht unterstützten Lichtfunktionen
Optionale Lichtfunktionen wie Farbtemperatur oder Transition dürfen die Grundfunktion nicht verhindern, wenn die verwendete Leuchte diese Funktion nicht unterstützt.

---

## 15. Dokumentation

### REQ-150 – Zweisprachige Beschreibung
Die Blueprint-Beschreibung soll die wesentlichen Funktionen in deutscher und englischer Sprache beschreiben.

### REQ-151 – README
Das Repository muss eine README mit Funktionsübersicht, Voraussetzungen, Prioritäten, Konfigurationshinweisen und Importinformationen enthalten.

### REQ-152 – Versionsinformation
Blueprint und Repository müssen eine nachvollziehbare Versionsinformation enthalten.

### REQ-153 – Changelog
Änderungen zwischen veröffentlichten Versionen müssen in einer `CHANGELOG.md` dokumentiert werden.
