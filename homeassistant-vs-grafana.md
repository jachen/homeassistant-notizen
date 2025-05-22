
# Warum unterscheiden sich Werte in Home Assistant und Grafana?

## 🧠 Grundprinzipien

1. **Home Assistant** zeigt **den aktuellen Zustand** (State) einer Entität.
2. **Grafana** zeigt **Zeitreihendaten** aus der Datenbank (z. B. InfluxDB), nicht den „Live“-Wert.

---

## 🔍 Typische Ursachen für unterschiedliche Werte

| Ursache                   | Beschreibung                                                    | Lösung                         |
|---------------------------|------------------------------------------------------------------|--------------------------------|
| `mean()` statt `last()`   | Durchschnitt statt aktuellem Wert                                | Verwende `last()`              |
| Fehlende Datenpunkte      | Sensor hatte `unavailable` oder Lücken                           | Query prüfen, Datenlücken suchen |
| Zeitbereich falsch        | HA zeigt „jetzt“, Grafana „Today so far“ oder 12h                | Zeitbereich auf z. B. „Last 5m“ stellen |
| Zeitzone abweichend       | Browser vs. UTC oder feste Einstellung                           | In Grafana rechts oben `browser` auswählen |
| Unterschiedliche Entitäten| HA zeigt Template-Sensor, Grafana rohen Originalsensor           | Quelle vergleichen             |
| Aggregierte Abfragen      | `GROUP BY time()` oder `fill()` verfälscht Live-Daten            | Aggregation entfernen oder feiner einstellen |

---

## ✅ Lösungsschritte in Grafana

1. Im **Query Editor** prüfen:
   ```sql
   SELECT last("value") FROM "autogen"."W"
   WHERE "entity_id" = 'sensor.power_total'
   ```
2. Zeitbereich oben rechts auf „**Last 5 minutes**“ stellen
3. Tooltip aktivieren → zeigt echten Rohwert zur Mausposition
4. Zeitzone prüfen: rechts oben → `browser`
5. Aggregationen wie `mean()`, `group by time()` vermeiden, wenn Live-Daten gebraucht werden

---

## 🧪 Beispiel: Was zeigt Home Assistant vs. Grafana?

### Home Assistant:
- `sensor.power_total` zeigt z. B. **-312 W**
- Wert wird live aktualisiert

### Grafana (mit `mean()`):
- zeigt möglicherweise **-185 W**, weil es den Schnitt über 5 Minuten bildet
- oder `No Data`, wenn Sensor kurz offline war

---

## 📌 Wichtig

- **Template-Sensoren** aus HA erscheinen nur in Grafana, wenn sie im `recorder` oder `influxdb:`-Include definiert sind.
- **Nullwerte oder negative Werte** können je nach Kontext (z. B. Solarleistung, Netzfluss) sinnvoll oder verwirrend sein – lieber mit eindeutigen Namen/Einheiten versehen.

---

## 💬 Zusammenfassung

- Home Assistant = Zustand in Echtzeit
- Grafana = Verlauf aus der Datenbank
- Unterschiede sind normal, aber **beherrschbar**
- Für Livewerte: `last()`, kein `mean()`, korrekte Zeitzone, keine Aggregation
