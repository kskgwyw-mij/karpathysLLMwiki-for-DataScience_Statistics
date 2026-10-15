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

## [2026-10-15] setup | Obsidian eingerichtet, Rohdateien auf den Stick
- Vault in Obsidian geöffnet; Community-Plugins Dataview (JavaScript Queries an) und Spaced Repetition aktiv.
- `.gitignore`: `.obsidian/plugins/` komplett ausgeschlossen (Plugin-Code nicht ins Repo); `community-plugins.json` wird versioniert.
- 20 Rohdateien aus der Ingest-Kopie (`~/repos/…/raw/`) nach `raw/` auf dem Stick kopiert; Namen wie in [[index]].
- Weiterhin offen: Prüfungsrelevanz Kapitel 5–6, Altklausuren (Jan fragt nach), Termin 03.07. vs. 13.11.

## [2026-10-15] setup | Klausurtermin bestätigt
- Jan: 13.11.2026, 11:00 ist der aktuelle Termin; der 03.07. aus VL1 F. 12 ist überholt.
- [[overview]] (Termin, Dauer, offene Fragen) und [[q-vl01-glm-basisfunktionen-splines]] angepasst.
- Offen bleibt nur die Dauer am 13.11.

## [2026-10-15] setup | README: Lernablauf
- README: neuer Abschnitt „Lernen“ (Lernsitzung in Obsidian → `quiz <thema>` im Terminal, tägliche Karteikarten), Verweis auf Lernplan in [[overview]].
- README „Einrichtung“: Obsidian Git nicht nötig, der Agent committet und pusht selbst.

## [2026-10-15] setup | README: Konzept LLM-Wiki
- README: neuer Abschnitt „Das Konzept: LLM-Wiki“ am Ende – Idee nach Karpathy, Mermaid-Diagramm des Kreislaufs (ingest → kompilieren → lernen → Frage/quiz/lint), Tabellen „Drei Schichten“ und „RAG und LLM-Wiki im Vergleich“.
- Diagramm vorab mit mermaid-cli gerendert und geprüft.

## [2026-10-15] setup | README: Diagramm als Bild
- Mermaid-Block in der README durch PNG ersetzt (Jan: GitHub zeigt das Flowchart nicht an); helle und dunkle Variante per `<picture>`.
- Bilder und Mermaid-Quelle in `.github/assets/llm-wiki-kreislauf*` (in Obsidian ausgeblendet).

## [2026-10-15] lint | Tabellen-Rendering in Obsidian
- Jan: Fairness-Tabelle in [[formelsammlung]] wurde als Rohtext angezeigt. Ursache: Tabelle direkt nach Absatzzeile ohne Leerzeile – Obsidian braucht eine Leerzeile vor Tabellen.
- Wikiweit 27 Stellen in 22 Dateien behoben (Leerzeile eingefügt, kein Inhalt geändert); Tabellen nach Überschriften waren korrekt.
- Normstriche in Tabellen-Mathe: `\|…\|` wird in Tabellenzellen zu `|…|` – auf `\lVert…\rVert` umgestellt in [[probabilistische-sicht-map]], [[regularisierung]], [[restringierte-optimierung]].
- Konvention für künftige Seiten: Leerzeile vor jeder Tabelle; in Tabellen keine `\|` in Formeln.

## [2026-10-15] setup | Eigene Ergänzungen grün markiert
- Jan: Inhalte, die nicht aus der VL stammen, sollen farblich erkennbar sein; dazu Beispielrechnungen und Anwendungsfälle auf allen Konzept- und Methodenseiten.
- Neue Callouts `[!beispiel]` (Rechenbeispiel, Anwendungsfall, Übungsaufgabe) und `[!ergaenzung]` (Erklärung, Herleitung, Lehrbuch), beide grün über `.obsidian/snippets/eigene-ergaenzung.css`.
- Bestand umgestellt: 38 `[!info]`-Ergänzungen → 9 `[!beispiel]`, 29 `[!ergaenzung]`; `[!info] Relevanz` bleibt (stützt sich auf die VL).
- AGENTS.md (Konvention „Eigene Ergänzungen") und Vorlagen konzept/methode angepasst.
- Offen: Batches A–E mit neuen Beispielen und Abgleich unmarkierter Beispiele gegen die Folien.

## [2026-10-15] query | Beispiele: Regression und Grundlagen (Batch A)
- Je ein grünes Rechenbeispiel und ein Anwendungsfall auf [[statistisches-lernproblem]], [[lineare-regression]], [[logistische-regression]], [[basisfunktionen-regression]], [[splines-und-b-splines]], [[bias-varianz-tradeoff]], [[modellauswahl-vorgehen]].
- Abgleich mit Folien: Odds-Ratio-Tabelle in [[logistische-regression]] war eigenes Material ohne Markierung → jetzt `[!beispiel]`. Übrige Beispiele stammen aus VL1-Übungen bzw. NB1.
- Alle Zahlen mit Python nachgerechnet.

## [2026-10-15] query | Beispiele: Regularisierung und Validierung (Batch B)
- Grüne Rechenbeispiele und Anwendungsfälle auf [[regularisierung]], [[ridge-regression]], [[lasso-regression]], [[output-regularisierung]], [[dropout]], [[label-smoothing]], [[early-stopping]], [[train-val-test-split]], [[kreuzvalidierung]], [[probabilistische-sicht-map]].
- Neuer Abschnitt „Beispiel" auf [[output-regularisierung]] und [[train-val-test-split]] (fehlte).
- Abgleich: λ-Tabelle in [[ridge-regression]] war eigene Rechnung ohne Markierung → `[!beispiel]`. NB2-/NB4-Beispiele sind Kursmaterial.
- Alle Zahlen mit Python nachgerechnet.

## [2026-10-15] query | Beispiele: Resampling und Optimierung (Batch C)
- Grüne Rechenbeispiele und Anwendungsfälle auf [[bootstrap]], [[bagging]], [[optimierung-grundlagen]], [[gradientenabstieg]], [[sgd]], [[momentum-und-nesterov]], [[adaptive-optimierer]], [[newton-und-quasi-newton]], [[restringierte-optimierung]], [[initialisierung]].
- Neuer Abschnitt „Beispiel" auf sechs Seiten, wo er fehlte (bootstrap, optimierung-grundlagen, sgd, adaptive-optimierer, restringierte-optimierung, initialisierung).
- Abgleich: θ²-Tabellen in [[gradientenabstieg]] und [[momentum-und-nesterov]] sowie das Newton-Beispiel waren eigene Rechnungen ohne Markierung → `[!beispiel]`. Momentum-Tabelle um Nesterov-Spalten erweitert.
- Alle Zahlen mit Python nachgerechnet.
