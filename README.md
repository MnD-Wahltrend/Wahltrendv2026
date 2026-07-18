# Umfragetrend

Dieses Repository dient als Datenablage für das Umfragetrend-Tool. Alle Umfragen werden
von Hand über die Benutzeroberfläche eingetragen (kein automatisiertes Scraping) und
automatisch in die Datei [`umfragedaten.json`](./umfragedaten.json) synchronisiert.

## Einrichtung

1. **Repository vorbereiten**
   Leg dieses Repo an (privat oder öffentlich, wie du magst) und lade `umfragedaten.json`
   aus diesem Ordner als Startdatei hoch, oder lass sie beim ersten Verbinden automatisch
   vom Tool anlegen.

2. **Token erzeugen**
   Auf github.com unter *Settings → Developer settings → Personal access tokens →
   Fine-grained tokens* ein neues Token erstellen:
   - **Repository access:** nur dieses eine Repository auswählen
   - **Permissions:** *Contents* → *Read and write*
   - Keine weiteren Rechte vergeben

3. **Im Tool verbinden**
   Im Umfragetrend-Tool unter „GitHub-Speicherung":
   - Benutzer/Organisation, Repository-Name, Pfad (`umfragedaten.json`) und Branch (`main`) eintragen
   - Token einfügen
   - optional „Token merken" aktivieren, damit sich das Tool künftig automatisch verbindet
   - auf **Verbinden** klicken

Ab diesem Zeitpunkt wird jede Änderung im Tool automatisch in diese Datei geschrieben.

## Datenformat (`umfragedaten.json`)

```jsonc
{
  "parties": [
    { "id": "cdu", "name": "CDU/CSU", "color": "#B9BEC7", "active": true }
    // ... weitere Parteien
  ],
  "polls": [
    {
      "id": "eindeutige-id",
      "institute": "Forsa",
      "date": "2026-07-18",
      "values": { "cdu": 28.5, "spd": 15.0, "gruene": 12.0 }
    }
    // ... weitere Umfragen
  ],
  "instituteWeights": {
    "Forsa": 1.2
    // Institut -> frei gewählter Gewichtungsfaktor, 1.0 = neutral
  },
  "halfLifeDays": 21
}
```

- **parties**: Liste der Parteien mit Farbe und ob sie aktuell aktiv (also in Formular,
  Tabelle und Chart sichtbar) ist.
- **polls**: jede einzelne erfasste Umfrage mit Institut, Datum und den Prozentwerten je Partei.
- **instituteWeights**: der von Hand vergebene Gewichtungsfaktor je Institut. Werte über 1.0
  zählen stärker in den Trend, Werte darunter schwächer.
- **halfLifeDays**: Anzahl Tage, nach denen eine Umfrage die Hälfte ihres Gewichts verliert
  (Aktualitäts-Gewichtung).

## Versionierung

Da die Datei bei jeder Änderung per GitHub-API committet wird, entsteht automatisch eine
Historie aller bisherigen Stände über den Commit-Verlauf dieser Datei — praktisch, um
frühere Trendstände nachzuvollziehen oder eine Änderung rückgängig zu machen.
