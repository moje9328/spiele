# Pixelhafen – Arbeitsanleitung für neue Spiele

Pixelhafen ist eine Sammlung kostenloser Retro-Spiele im Browser (Sprache: **Deutsch**, Du-Form).
Diese Seite (`~/spiele-seite`, Repo `moje9328/spiele`) ist nur die **Karten-Übersicht**. Jedes Spiel ist ein eigenes Projekt.

## Wo was liegt

| Was | Ort |
|---|---|
| Übersichtsseite | `~/spiele-seite/index.html` (Liste `SPIELE`, Screenshots in `bilder/`) |
| Spiele | `~/<name>-app/` – je ein eigenes Git-Repo `moje9328/<name>`, online über GitHub Pages (main, Ordner `/`) |
| Online-Adresse | `https://moje9328.github.io/<name>/` |

Bestehende Spiele: froschsprung, hafenflitzer, hafenhopser, hafenlotse, krabbenalarm, krabbenkrawall, nachtschicht, orbitflipper, pinball, sonnenrutsch, containerstapler, steinbrecher, sternenwacht, wolkenhuepfer, wurmgrube.

## Aufbau eines Spiels

- **Eine einzige `index.html`**, kein Build-Tool, keine Bibliotheken, Schrift „Press Start 2P“ eingebettet. Läuft offline.
- Dazu: `manifest.webmanifest` (installierbare App), `sw.js` (Offline-Cache), `icons/` (192, 512, apple-touch).
- Größere Spiele (Sonnenrutsch, Krabben-Krawall, Froschsprung, Hafenflitzer, Hafenhopser, Orbitflipper) haben `quelle/` mit Teilen (`1-einstellungen.js` … `9-app.js`, `kopf.html`, `basis.css`) und `quelle/baue.py`, das daraus die `index.html` baut. **Dann `index.html` nie direkt bearbeiten**, sondern `quelle/` ändern und `python3 quelle/baue.py` ausführen.
- Vorbild für den Aufbau, Tests und Entscheidungen: `~/sonnenrutsch-app/SESSION_HANDOFF.md` und `~/krabbenkrawall-app/SESSION_HANDOFF.md`. **Vor Arbeit an einem Spiel dessen Handoff lesen.**
- `basis.css` ist das gemeinsame Pixelhafen-Design. Kalle die Krabbe (`malKrabbe`) wird wörtlich aus Krabbenalarm übernommen.
- Speicher: `localStorage` unter `pixelhafen.<name>` mit Versionsnummer, immer try/catch (kaputter oder gesperrter Speicher darf nichts zerstören).

## Qualitätsmaßstab (so wurden die bisherigen Spiele gebaut)

- Handy zuerst (Touch, Pointer Events), dazu Tastatur/Maus; Pause mit P/Esc und bei Tabwechsel.
- Physik in festen Zeitschritten, unabhängig von der Bildrate.
- „Weniger Bewegung“ respektieren, Töne nur nach Nutzeraktion und abschaltbar.
- Fair: Jedes Level mit Testbot als schaffbar nachweisen.
- Eigene Tests: `node quelle/test-logik.js` (Logik) und `python3 quelle/test-browser.py` (Chromium; Argument `webkit` = Safari-Motor). Neue Funktionen bekommen Tests; ein Fix für einen Fehler bekommt einen Test, der ihn abfängt.
- Ehrlich berichten, was **nicht** geprüft wurde (echtes Handy, Ton hörbar, 120 Hz).
- Texte im Spiel auf Deutsch, freundlich, kurz.

## Neues Spiel anlegen

1. Ordner `~/<name>-app` kopieren/anlegen nach dem Muster eines ähnlichen Spiels, `git init`, Repo `moje9328/<name>` auf GitHub, Pages aktivieren.
2. `manifest.webmanifest`, Icons, `sw.js` mit eigenem `CACHE`-Namen (`<name>-v1`).
3. Screenshots (3 Stück, JPG) nach `~/spiele-seite/bilder/<name>-*.jpg`.
4. In `~/spiele-seite/index.html` einen Eintrag in `SPIELE` ergänzen (Vorlage steht am Ende der Liste): `titel`, `beschreibung`, `merkmale`, `steuerung`, `spielen`, `download`, `dateiname`, `bilder`.
5. `SESSION_HANDOFF.md` im neuen Spiel anlegen (Stand, Wunsch, Änderungen, Bauen/Prüfen, offene Punkte).

## Spiel aktualisieren

1. Änderung in `quelle/` (oder `index.html`, falls kein `quelle/`), bauen, Tests laufen lassen.
2. `sw.js`: `CACHE` hochzählen (z. B. `sonnenrutsch-v10` → `v11`), sonst sehen Spieler die alte Version.
3. In `SESSION_HANDOFF.md` eine neue Runde eintragen (Wunsch, Ursache, Änderungen, Tests, Nicht-Geprüftes).
4. Committen (deutsche, beschreibende Nachricht) und **erst nach Rückfrage des Nutzers pushen**.
5. Hat sich das Aussehen geändert: Screenshot und Beschreibung in `~/spiele-seite` anpassen.

## Vorsicht

- In `~/spiele-seite` nur **gezielt stagen** (`git add <datei>`), nie `git add -A`: Dort können unfertige Änderungen (z. B. Bewertungen, Hafenlotse) liegen, die nicht mit hochdürfen.
- Krabbenalarm und andere fertige Spiele nicht nebenbei verändern.
- Pushen veröffentlicht live. Nur auf ausdrücklichen Wunsch.
