# 🕐 Uhrenstadt — Uhrzeit lernen & Zeitspannen berechnen

Ein Lernspiel zum Ablesen der Uhr und Rechnen mit Zeitspannen, verpackt als
freundliche kleine Stadt mit Schule, Spielplatz und Tagesablauf.

**Zielgruppe:** Grundschule
**Status:** Version 1.0

## 🎮 Die neun Modi

1. **Wie spät ist es?** — analoge Uhr ablesen, digitale Uhrzeit auswählen
2. **Stell die Uhr!** — Zeiger per Ziehen oder +/-Buttons selbst einstellen
   (das Herzstück des Spiels)
3. **Welche Uhr stimmt?** — zur digitalen Zeit die passende analoge Uhr finden
4. **Analog & digital** — beide Richtungen gemischt
5. **Vormittag oder Nachmittag?** — Tageskontext + 24-Stunden-Zeit verstehen
6. **Wie viel Zeit vergeht?** — Zeitspanne zwischen zwei Uhrzeiten berechnen
7. **Wie spät ist es dann?** — vorwärts rechnen
8. **Wie spät war es vorher?** — rückwärts rechnen
9. **Mein Tagesablauf** — fünf Aufgabentypen rund um Alltagssituationen
   (Uhrzeit zuordnen, Reihenfolge, Vergleich, Zeitspanne)

Die meisten Modi bieten 3–5 Schwierigkeitsstufen (volle Stunden bis
minutengenau, bzw. leicht/mittel/schwer bei Zeitspannen).

Eine Runde besteht aus 8 richtig gelösten Aufgaben (nicht 8 Versuche),
sichtbar als kleine Uhren in der Fortschrittsanzeige. Keine Zeitbegrenzung,
kein Punktabzug, kein Game Over.

## 🕐 Die analoge Uhr — zentrale Komponente

Eine einzige, zentral verwendete Uhr-Funktion berechnet die Zeigerstellung
für **alle** Uhren im Spiel nach der mathematisch korrekten Formel:

```
Minutenzeiger: minute * 6
Stundenzeiger: (stunde % 12) * 30 + minute * 0.5
```

Der Stundenzeiger bleibt dadurch **nie** starr auf einer Stundenzahl stehen,
sondern wandert bei 3:30 sichtbar zwischen 3 und 4, bei 8:45 deutlich
Richtung 9, bei 11:55 fast bis zur 12 — genau wie eine echte Uhr.

**12/24-Stunden-Logik:** 14:30 und 2:30 zeigen auf einer analogen Uhr
dieselbe Zeigerstellung. Beim interaktiven Einstellen wird deshalb beim
Prüfen nur die Zeigerstellung verglichen (Stunde mod 12 + Minute) — eine
auf 2:30 eingestellte Uhr wird für die Aufgabe „Stelle 14:30 Uhr ein"
korrekt als richtig akzeptiert. Ohne Tageskontext wird nie verlangt, allein
anhand einer analogen Uhr zwischen z. B. 7:00 und 19:00 zu unterscheiden.

## 🛠️ Technik

Eine einzige, in sich geschlossene `index.html`-Datei — kein Build-Prozess,
keine Abhängigkeiten, kein Server nötig.

- Reines HTML, CSS und JavaScript (kein Framework)
- Uhrzeiten werden intern durchgängig als **Minuten seit 00:00** gespeichert
  und verrechnet (`timeToMinutes()`, `minutesToTime()`, `addMinutes()`,
  `subtractMinutes()`, `durationBetween()`) — keine Datumsobjekte, keine
  Rundungsfehler
- Die interaktive Uhr aktualisiert beim Ziehen des Minutenzeigers gezielt
  nur die Zeiger-Koordinaten im bestehenden SVG, statt die ganze Uhr bei
  jeder Mausbewegung neu aufzubauen — das verhindert, dass die
  Positionsberechnung nach der ersten Bewegung auf ein entferntes
  DOM-Element zeigt und dadurch hängen bleibt
- Distraktoren bei „Welche Uhr stimmt?" und den digitalen Auswahlaufgaben
  bilden typische Ablese-Fehler nach (Stunde ±1, 15↔45 vertauscht, 20↔40
  vertauscht), nicht beliebige Zufallszeiten
- Sprachausgabe optional über den Lautsprecher-Button (native
  `SpeechSynthesis`-API), Uhrzeiten werden dabei in gesprochener Form
  wiedergegeben (z. B. „vierzehn Uhr dreißig")
- Sicherheitsnetz: löst sich ein interner Sperr-Zustand durch einen
  unerwarteten Fehler nicht rechtzeitig, wird er automatisch freigegeben
- Läuft vollständig offline, keine externen Ressourcen, keine Cookies,
  kein Tracking
- Responsiv für Smartboard, Desktop, Tablet und Smartphone

## 📁 Projektstruktur

```
uhrenstadt-projekt/
├── index.html      ← das komplette Spiel
├── README.md
└── .gitignore
```

## ▶️ Lokal ausprobieren

Einfach `index.html` im Browser öffnen — kein Server, keine Installation
nötig.

## 🌐 Veröffentlichung

### GitHub
1. Neues Repository auf [github.com](https://github.com) anlegen.
2. `index.html`, `README.md` und `.gitignore` hochladen.

### Netlify (per Drag & Drop)
1. Auf [app.netlify.com](https://app.netlify.com) einloggen.
2. Den Projektordner direkt in den Browser ziehen („Deploy manually").
3. Netlify vergibt sofort einen Link. Build command: leer, Publish
   directory: `.`

## ✅ Qualitätssicherung

- **Alle in der Spezifikation genannten Zeigerstellungen** (Abschnitt 27)
  einzeln mathematisch UND visuell geprüft: 12:00, 3:00, 6:00, 9:00 exakt;
  3:15/3:30/3:45, 8:15/8:30/8:45, 11:30/11:45/11:55 alle korrekt zwischen
  den Stundenzahlen positioniert
- **12/24-Stunden-Äquivalenz** (Abschnitt 29) bestätigt: 7:00↔19:00,
  8:30↔20:30, 14:45↔2:45 erzeugen identische Zeigerwinkel
- **Alle Zeitrechnungs-Testfälle** (Abschnitt 28) nachgerechnet: Dauer-
  sowie Vorwärts-/Rückwärts-Berechnungen stimmen exakt mit den
  vorgegebenen Beispielen überein
- Über 700 automatisiert erzeugte Aufgaben über alle 9 Modi und sämtliche
  Schwierigkeitsstufen geprüft: 0 Fehler bei Zeitberechnung, Distraktor-
  Eindeutigkeit, Optionsanzahl und Lösbarkeit
- **Ein echter Fehler beim Testen gefunden und behoben:** Bei „Vormittag
  oder Nachmittag?" erzeugte die Situation „isst zu Mittag" (12:00) durch
  einfache +12-Rechnung eine unsinnige Option „24:00 Uhr" statt korrekt
  umzubrechen — behoben durch korrekte Modulo-Rechnung und Anpassung der
  Situation auf eine Uhrzeit ohne diesen Sonderfall
- Die interaktive Uhr („Stell die Uhr!") mit allen 19 Modus/Stufen-
  Kombinationen über volle Runden (8/8) mit echten Klicks getestet,
  einschließlich Drag-Bedienung des Minutenzeigers und Stundenübergang
  beim Überschreiten der vollen Stunde
- Alle 6 vorgeschriebenen Bildschirmgrößen (375×667 bis 1920×1080)
  geprüft: kein horizontales Scrollen, Uhr und Buttons vollständig sichtbar

## 🖨️ Arbeitsmaterialien

Im Ordner `arbeitsblaetter/` (jeweils als HTML und PDF):

- `teilnehmeruebersicht` — A4 quer, 30 Kinder × 10 Runden zum Abhaken
- `arbeitsblatt-uhrzeit-lesen` — Uhrzeit ablesen, Zeiger einzeichnen, passende Uhr
  auswählen, Vormittag/Nachmittag (mit Tageskontext)
- `arbeitsblatt-zeitspannen` — Zeitspanne berechnen, vorwärts und rückwärts rechnen,
  Tagesablauf (Reihenfolge, Zeitspanne)

Alle Uhren auf den Blättern verwenden dieselbe Zeigerformel wie das Spiel; ohne
Tageskontext wird nie zwischen z. B. 7:00 und 19:00 unterschieden.

## 📄 Lizenz

© Förderfreude Games. Alle Rechte vorbehalten.
