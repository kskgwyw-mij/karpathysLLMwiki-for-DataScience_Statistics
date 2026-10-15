# Log

> Append-only. Jeder Eintrag beginnt mit `## [JJJJ-MM-TT] <ingest|query|quiz|lint|setup> | Titel`.
> Letzte Einträge: `grep "^## \[" wiki/log.md | tail -5`

## [2026-10-14] setup | Vault-Gerüst angelegt
- Schema (AGENTS.md, CLAUDE.md), index, log, overview, dashboard, formelsammlung, schwachstellen, Templates
- Repo öffentlich auf GitHub; `raw/` per .gitignore ausgeschlossen (Urheberrecht)
- Offen: Modul und Klausurformat bestätigen, Vorlesungsunterlagen nach `raw/vorlesung/` legen, erster Ingest

## [2026-10-15] ingest | Moodle-Export Statistical Learning 2 (Batch)
- Batch-Ingest aller 22 Dateien (Jan: „hier sind die Dateien, die als Input einfließen sollen"); umbenannt nach `raw/vorlesung`, `raw/uebungen`, `raw/literatur` (Mapping im [[index]]). Unterordner `raw/uebungen/daten/` und `raw/uebungen/modellauswahl/` neu.
- 17 Quellenseiten, 10 Konzepte, 28 Methoden, 13 Aufgaben, 4 Vergleiche; [[formelsammlung]], [[overview]] (Prüfungsmodus, Themenkarte, Lernplan), [[index]] neu geschrieben.
- Relevanzregel: Folienauswahl „essentiell" = hoch, übrige VL = mittel, Vertiefung/„nur für Interessierte" = niedrig. ESL nur als Kapitelzuordnung, nicht vollständig ingested.
- Folienfehler dokumentiert: Newton-Update ohne $\theta_t$ (VL0 F. 91, VL4 F. 55), Dropout-Text auf Label-Smoothing-Folie, PDP-Formel ohne $1/N$; Bugs in NB3 (Early Stopping nie aktiv).
- Handrechnungen mit Code verifiziert; drei Notebooks in `notebooks/` erzeugt und ausgeführt.
- Offen: Prüfungsrelevanz Kapitel 5–6, Termin 03.07. vs. 13.11., Altklausuren.

## [2026-10-15] setup | Schema: Push nach jedem Commit
- AGENTS.md, Abschnitt Git: nach jedem Commit `git push origin main` ohne Rückfrage; bei abgelehntem Push kein Force-Push, sondern Jan informieren (auf Jans Wunsch).
- git nutzt die gh-Anmeldung (`gh auth setup-git`).
