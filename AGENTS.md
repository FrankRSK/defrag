# Defrag Bildschirmschoner - Projektanweisungen

## Projektübersicht

Dies ist eine nostalgische Bildschirmschoner-Simulation, die das klassische Windows-Defragmentierungsprogramm nachahmt. Das Projekt besteht aus einer einzelnen HTML-Datei mit eingebettetem CSS und JavaScript. Es simuliert visuell eine Festplatten-Defragmentierung mit authentischen Festplattengeräuschen.

## Entwicklung

### Ausführung

```bash
# Projekt im Browser öffnen (entweder direkt oder via lokalem Server)
firefox "defrag (Claude) 0.04.html"
google-chrome "defrag (Claude) 0.04.html"

# Oder mit einem einfachen HTTP-Server:
python -m http.server 8000
# Dann öffnen: http://localhost:8000
```

### Linting und Formatierung

Da es sich um ein reines HTML-Projekt ohne Build-System handelt, gibt es keine automatischen Lint-Tools. Empfehlungen für manuelle Prüfung:

```bash
# HTML-Validierung (optional)
npx html-validate "defrag (Claude) 0.04.html"

# JavaScript-Syntax prüfen (optional)
npx eslint --no-eslintrc --parser-options=ecmaVersion:2020 "defrag (Claude) 0.04.html"
```

### Tests

Es gibt keine automatisierten Tests. Manuelles Testen:

1. Datei im Browser öffnen
2. Auf Vollbildmodus prüfen (Klick)
3. ESC zum Verlassen des Vollbildmodus
4. Audiowiedergabe nach erstem Klick prüfen
5. Defragmentierungsanimation beobachten
6. Verschiebbare Legende testen (Drag & Drop)
7. Fenstergröße ändern und Neustart der Simulation prüfen

## Code-Stil-Richtlinien

### HTML

- **Dokumenttyp**: HTML5 mit `<!DOCTYPE html>`
- **Sprache**: `lang="de"` für deutsche Benutzeroberfläche
- **Zeichencodierung**: UTF-8
- **Viewport**: Responsive mit `width=device-width, initial-scale=1.0`
- **Einrückung**: 4 Leerzeichen
- **Klassennamen**: BEM-ähnliche Konvention mit Präfixen
  - Block-Typen: `block-free`, `block-fragmented`, `block-mft`, etc.
  - Komponenten: `disk-map`, `title-bar`, `status-bar`, `legend-item`

### CSS

- **Einrückung**: 4 Leerzeichen
- **Selektoren**: Klassenselektoren bevorzugen, ID-Selektoren nur für JavaScript-Hooks
- **Farben**: Hex-Farben (`#c0c0c0`) oder benannte Farben
- **Klassennamenskonvention**:
  ```css
  .block-{typ}     /* Block-Zustände */
  .{komponente}    /* Komponenten-Container */
  .{komponente}-{element}  /* Unterelemente */
  ```
- **CSS-Variablen**: Werden aktuell nicht verwendet, können für zukünftige Änderungen eingeführt werden
- **Animationen**: `@keyframes` für wiederverwendbare Animationen definieren
- **Wichtig**: `!important` verwenden bei Block-Typ-Klassen, um Überschreibungen zu verhindern

### JavaScript

- **Einrückung**: 4 Leerzeichen
- **Variablendeklaration**:
  - `let` für veränderliche Variablen
  - `const` für Konstanten
  - Keine `var`-Verwendung mehr
- **Funktionen**:
  - `function`-Deklarationen für Hauptfunktionen
  - Arrow-Funktionen für Callbacks und Event-Handler
  - `async/await` für asynchrone Operationen
- **Strings**:
  - Template-Literale für interpolierte Strings: `` `Text ${variable}` ``
  - Einzelne Anführungszeichen für einfache Strings: `'string'`
- **DOM-Manipulation**:
  - `document.getElementById()` für einzelne Elemente
  - `classList.add/remove/contains` für Klassenmanipulation
  - `createElement()` für neue Elemente
- **Event-Handler**:
  - `addEventListener` bevorzugen
  - Inline-Event-Handler vermeiden
- **Promises**: `Promise` mit `setTimeout` für Delays
- **Fehlerbehandlung**: `try/catch` bei kritischen Operationen (z.B. AudioContext)

### Kommentare

- **Keine Kommentare im Code**: Der Code soll selbsterklärend sein
- **Ausnahme**: Falls absolut notwendig, deutsche Kommentare für komplexe Logik

### Benennungskonventionen

- **Variablen und Funktionen**: camelCase
  ```javascript
  let blocksPerRow = 0;
  function findNextFragmentedBlock() { }
  ```
- **CSS-Klassen**: kebab-case
  ```css
  .disk-map { }
  .block-fragmented { }
  ```
- **IDs**: kebab-case
  ```html
  <div id="blockGrid">
  ```
- **Konstanten**: UPPER_SNAKE_CASE für echte Konstanten (optional)
- **Private Funktionen**: Keine explizite Kennzeichnung, aber `_`-Präfix möglich

### Dateinamen

- Hauptdatei: `defrag (Claude) {version}.html`
- Archiv: `Archiv/` für ältere Versionen
- Ressourcen: `Sounds/` für Audiodateien, `icon.png` für Favicon

## Projektstruktur

```
Defrag/
├── defrag (Claude) 0.04.html  # Hauptdatei (aktuelle Version)
├── AGENTS.md                   # Diese Datei
├── info.txt                    # Projektbeschreibung
├── icon.png                    # Icon
├── Defrag Verbesserungen.odt   # Verbesserungsideen
├── Archiv/                     # Ältere Versionen
│   ├── defrag (Claude) 0.04.html
│   ├── defrag (Claude) 0.1.html
│   ├── defrag (Claude) 0.2.html
│   └── defrag (Claude) 0.3.html
└── Sounds/                     # Audiodateien (optional)
```

## Wichtige Hinweise

### Block-Typen und Farben

| Typ | Farbe | Beschreibung |
|-----|-------|--------------|
| `free` | #d3d3d3 | Freier Speicherplatz |
| `unfragmented` | #00ff00 | Nicht fragmentiert |
| `mft` | #800080 | Master File Table |
| `immovable` | #696969 | Nicht verschiebbar |
| `processing` | #ffa500 | In Bearbeitung |
| `fragmented` | #ff0000 | Fragmentiert |
| `defragmented` | #0000ff | Defragmentiert |
| `reading` | #ffff00 | Wird gelesen |
| `writing` | #ff00ff | Wird geschrieben |
| `temp-storage` | #00ffff | Zwischenspeicher |

### Audio-Implementation

- AudioContext wird beim ersten Klick initialisiert (Browser-Policy)
- Drei Sound-Typen: `seek`, `read`, `write`
- Web Audio API mit Oscillator für synthetische Geräusche

### Defragmentierungsphasen

1. **Phase 1**: Analysiere und sammle Fragmente
2. **Phase 2**: Ordne Dateien neu an
3. **Phase 3**: Konsolidiere freien Speicher

### Vollbildmodus

- Klick aktiviert Vollbildmodus
- ESC beendet Vollbildmodus
- Cursor wird ausgeblendet (`cursor: none`)

## Häufige Aufgaben

### Neue Block-Typen hinzufügen

1. CSS-Klasse definieren: `.block-{name} { background-color: {farbe}; }`
2. In `createBlock()` und `setBlockType()` integrieren
3. Legende in HTML erweitern

### Defragmentierungsgeschwindigkeit ändern

- `delay()`-Aufrufe in `moveFragmentedFile()` anpassen
- Hauptloop-Delay in `startDefragmentation()` anpassen

### Neue Phasen hinzufügen

1. `phases`-Objekt erweitern
2. `completePass()`-Logik anpassen
3. Fortschrittsberechnung aktualisieren

## Sprache

- **UI-Texte**: Deutsch
- **Code-Kommentare**: Keine (oder Deutsch falls notwendig)
- **Variablennamen**: Englisch
- **Funktionsnamen**: Englisch
