# Statistik-Wiki – Schema für den Wiki-Agenten

Dieses Dokument ist die Arbeitsanweisung für jeden LLM-Agenten (Claude Code, Codex, Pi, …), der dieses Vault pflegt. Es folgt dem LLM-Wiki-Muster von Andrej Karpathy: Rohquellen werden **einmal** in ein persistentes, querverlinktes Markdown-Wiki kompiliert und danach aktuell gehalten, statt bei jeder Frage neu aus den Quellen abgeleitet zu werden.

**Obsidian ist die IDE, der Agent ist der Programmierer, das Wiki ist die Codebasis.**

## Zweck

Lern-Wiki für die **Statistik-Klausur am Fr, 13.11.2026, 11:00 Uhr** (M.Eng. Intelligent Analytics & Data Science, FH Kufstein Tirol).

- **Du (Agent)** schreibst und pflegst `wiki/` vollständig: Zusammenfassungen, Konzept- und Methodenseiten, Formelsammlung, Querverweise, Übungsfragen, Index, Log.
- **Jan** kuratiert die Quellen, stellt Fragen, prüft und entscheidet, was prüfungsrelevant ist. Er editiert das Wiki selten von Hand.
- Ziel: Alles Prüfungsrelevante aus den Quellen sauber kompiliert, mit Intuition, Formeln, Rechenbeispielen, typischen Fallstricken und Karteikarten – so, dass Jan in den letzten Tagen nur noch das Wiki braucht.

## Schichten

| Schicht | Inhalt | Wer schreibt |
|---|---|---|
| `raw/` | Unveränderliche Quellen: Folien, Skripte, Übungsblätter, Lösungen, Altklausuren, Literatur | Jan legt ab. Agent **liest nur**, ändert und löscht nie. |
| `wiki/` | Vom Agenten erzeugte Markdown-Seiten | Agent |
| `notebooks/` | Jupyter-Notebooks (Python) zu Methoden und Aufgaben | Agent, nach Absprache |
| `templates/` | Seitenvorlagen | Agent, nur nach Absprache |
| `AGENTS.md` | Dieses Schema | Nur gemeinsam mit Jan weiterentwickeln |

## Verzeichnisstruktur

```
raw/
  vorlesung/      Folien und Skripte (Dateiname mit Nummer, z. B. vl03-ridge-lasso.pdf)
  uebungen/       Übungsblätter und Lösungen
  altklausuren/   Probe- und Altklausuren
  literatur/      Lehrbuchkapitel, Paper, Web-Clips
  assets/         Bilder (Obsidian-Anhangsordner)
wiki/
  index.md        Inhaltskatalog – zuerst lesen
  log.md          Chronologisches Protokoll (append-only)
  overview.md     Klausur, Format, Themenkarte, Lernplan
  dashboard.md    Dataview-Fortschrittsansicht (nicht bearbeiten außer auf Wunsch)
  formelsammlung.md
  quellen/        Eine Seite pro Rohquelle (Präfix q-)
  konzepte/       Grundbegriffe und Zusammenhänge (z. B. bias-varianz-tradeoff)
  methoden/       Verfahren (z. B. ridge-regression, k-fold-kreuzvalidierung)
  aufgaben/       Durchgerechnete Aufgaben mit Lösungsweg
  vergleiche/     Gegenüberstellungen und abgelegte Query-Antworten
  lernen/         schwachstellen.md, Quiz-Ergebnisse
notebooks/        .ipynb, Dateiname = Wiki-Seite (z. B. ridge-regression.ipynb)
templates/        Vorlagen für Konzept-, Methoden-, Quellen- und Aufgabenseiten
```

## Seitenformat

### Frontmatter (Pflicht auf jeder Seite in `wiki/` außer index, log, dashboard)

```yaml
---
typ: konzept            # konzept | methode | quelle | aufgabe | vergleich | uebersicht
themen: [regression]    # freie, aber konsistente Themen-Tags
aliases: []             # Synonyme, englische Begriffe
quellen: ["[[q-vl03-ridge-lasso]]"]
relevanz: hoch          # hoch | mittel | niedrig  (Prüfungsrelevanz)
beherrschung: neu       # neu | unsicher | sicher  (Jans Stand; nur nach Quiz oder auf Jans Ansage ändern)
aktualisiert: 2026-10-14
---
```

Keys bleiben ASCII (Dataview). `relevanz` leitest du aus Folien-Hinweisen („klausurrelevant", Wiederholungen, Übungsblätter, Altklausuren) ab; im Zweifel `mittel` und Jan fragen.

### Aufbau von Konzept- und Methodenseiten

1. **Kurzdefinition** – 1–2 Sätze, klausurtauglich formuliert
2. **Intuition** – warum es das gibt, welches Problem es löst
3. **Formal** – Definitionen, Formeln, Herleitungsskizze; Notation wie in der Vorlesung
4. **Annahmen & Voraussetzungen**
5. **Beispiel** – kleines Rechenbeispiel oder Interpretation eines R/Python-Outputs; Link auf Notebook, falls vorhanden
6. **Typische Prüfungsfragen & Fallstricke**
7. **Verwandt** – Wikilinks mit je einem Halbsatz, *wie* es zusammenhängt
8. **Karteikarten**
9. **Quellen** – mit Folien- bzw. Seitenangabe

## Konventionen

- **Dateinamen:** kebab-case, ASCII (ae/oe/ue/ss), sprechend: `wiki/methoden/lasso-regression.md`. Quellenseiten: `wiki/quellen/q-<rohdatei-ohne-endung>.md`.
- **Links:** Obsidian-Wikilinks `[[seite]]`, `[[seite#Abschnitt]]`, `[[seite|Anzeigetext]]`. Jede neue Seite bekommt mindestens einen eingehenden und einen ausgehenden Link.
- **Formeln:** `$…$` inline, `$$…$$` als Block (MathJax). Notation der Vorlesung übernehmen. Weichen Quellen voneinander ab (z. B. Lehrbuch vs. Folien), auf der Seite in einem Callout `> [!note] Notation` gegenüberstellen.
- **Belege:** Jede inhaltliche Aussage ist auf eine Quelle zurückführbar: `(→ [[q-vl03-ridge-lasso]], Folie 12)`. Nichts erfinden. Ergänzungen, die nicht aus den Quellen stammen, als `> [!info] Ergänzung (nicht aus der VL)` kennzeichnen.
- **Widersprüche** zwischen Quellen oder zu bestehenden Seiten: `> [!warning] Widerspruch` mit beiden Positionen und Belegen; nicht stillschweigend auflösen.
- **Sprache:** Deutsch. Fachbegriffe so, wie die Vorlesung sie verwendet; bei Erstnennung den englischen bzw. deutschen Gegenbegriff in Klammern und als Alias.
- **Terminologie:** Für die unauffällige Klasse (Klassifikation, Anomalieerkennung, Netzwerkverkehr) ausschließlich „gutartig" oder „unauffällig" schreiben – nie das englische Wort für „gutartig" und nie dessen eingedeutschte Form.
- **Karteikarten** im Format des Obsidian-Plugins *Spaced Repetition*: Abschnitt `## Karteikarten`, Tag `#flashcards/statistik` auf der Seite, einzeilig `Frage::Antwort`, mehrzeilig Frage – Zeile mit `?` – Antwort. Pro Seite 3–8 Karten, Fokus auf Prüfungsfragen.
- **Callouts** für Merksätze: `> [!tip] Merke`.

## Operationen

### Ingest
Auslöser: „ingest raw/…", „neue Quellen verarbeiten".

1. Quelle vollständig lesen (PDF inkl. Formeln; bei Folien mit Grafiken die relevanten Abbildungen ansehen).
2. Kernaussagen kurz mit Jan besprechen und fragen, was er betont haben will – außer er sagt „batch".
3. Quellenseite `wiki/quellen/q-….md` anlegen: Zusammenfassung, Gliederung mit Folienbereichen, prüfungsrelevante Aussagen, offene Fragen.
4. Konzept- und Methodenseiten anlegen oder aktualisieren; bestehende Inhalte ergänzen statt duplizieren.
5. `wiki/formelsammlung.md` ergänzen (Formel + Einzeiler + Link zur Seite).
6. Themenkarte in `wiki/overview.md` und `wiki/index.md` aktualisieren.
7. Log-Eintrag schreiben.

Eine Quelle berührt typischerweise 5–15 Seiten. Lieber eine Quelle gründlich als drei oberflächlich.

### Query
1. Zuerst `wiki/index.md` lesen, dann die relevanten Seiten; `raw/` nur bei Lücken.
2. Antwort mit Wikilinks belegen.
3. Gute Antworten (Vergleich, Herleitung, Rechenweg) nach kurzer Rückfrage als Seite in `wiki/vergleiche/` oder `wiki/aufgaben/` ablegen, verlinken, Index und Log aktualisieren. Erkenntnisse sollen sich im Wiki ansammeln, nicht im Chatverlauf verschwinden.

### Quiz
Auslöser: „quiz <thema>", „prüf mich".
1. Fragen aus Seiten mit `relevanz: hoch` und `beherrschung` ≠ `sicher` ziehen; Mischung aus Verständnis, Rechnen und Output-Interpretation, Stil wie Altklausuren (falls vorhanden).
2. Eine Frage nach der anderen; erst nach Jans Antwort auflösen und erklären.
3. Danach `beherrschung` der betroffenen Seiten anpassen, Fehlerbilder in `wiki/lernen/schwachstellen.md` eintragen, Log-Eintrag.

### Lint
Auslöser: „lint" – mindestens wöchentlich und in der Woche vor der Klausur.
Prüfen: Widersprüche, veraltete Aussagen, verwaiste Seiten, erwähnte Begriffe ohne eigene Seite, fehlende Querverweise, Aussagen ohne Beleg, uneinheitliche Notation, Altklausur-Themen ohne Wiki-Abdeckung, Seiten ohne Karteikarten.
Ergebnis als priorisierte Liste an Jan und Log-Eintrag; Korrekturen nach Freigabe.

## index.md

Inhaltlicher Katalog, nach Kategorien gegliedert. Pro Seite eine Zeile:
`- [[seite]] — Einzeiler · relevanz · Quellen: n`
Bei jedem Ingest und jeder neuen Seite aktualisieren. Bei Queries immer zuerst lesen.

## log.md

Append-only. Jeder Eintrag beginnt mit
`## [JJJJ-MM-TT] <ingest|query|quiz|lint|setup> | Titel`
gefolgt von 1–5 Stichpunkten (berührte Seiten, offene Punkte). So bleibt das Log mit `grep "^## \[" wiki/log.md | tail -5` auswertbar.

## Git

Das Vault ist ein **öffentliches** GitHub-Repo.

- `raw/` ist per `.gitignore` ausgeschlossen (Urheberrecht an Folien, Übungen, Altklausuren). Nur die leere Ordnerstruktur wird versioniert. Niemals Rohquellen force-adden oder Inhalte daraus wörtlich in längeren Passagen ins Wiki kopieren – zusammenfassen, eigene Formulierungen, Formeln und Beispiele sind in Ordnung.
- Keine personenbezogenen Daten Dritter (Mitstudierende, Lehrende über Name und Rolle hinaus) und keine dienstlichen Inhalte ins Wiki.
- Nach jeder abgeschlossenen Operation ein Commit, Nachricht im Log-Format: `ingest: vl03-ridge-lasso`, `quiz: kreuzvalidierung`, `lint: 2026-10-21`.
- Direkt danach `git push origin main`, ohne Rückfrage. Wird der Push abgelehnt, nie force-pushen, sondern Jan Bescheid geben.

## Grenzen

- `raw/` niemals verändern oder löschen.
- Keine Wiki-Seiten löschen; Überholtes mit `status: ersetzt` und Link auf die Nachfolgeseite markieren.
- `beherrschung` nie eigenmächtig auf `sicher` setzen – nur nach Quiz oder auf Jans Ansage.
- Bei Unklarheit, was prüfungsrelevant ist oder wie etwas gemeint ist: fragen statt raten.
- Dieses Schema nur nach Absprache ändern; Änderungen im Log vermerken.
