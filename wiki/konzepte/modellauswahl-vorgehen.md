---
typ: konzept
themen: [modellwahl]
aliases: [Modellauswahl, Wie wähle ich die passende ML Methode, Model Selection, Faustregeln Modellwahl]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl02-bias-varianz-modellwahl]]", "[[q-lernhilfe-sl2]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-16
---

# Modellauswahl: Welche Methode für welches Problem?

#flashcards/statistik

## Kurzdefinition
Leitfaden der Vorlesung, um für ein konkretes Problem eine begründete Modellfamilie zu wählen: **Problem verstehen und klassifizieren → Datenlage prüfen → Zusatzanforderungen klären**, umgesetzt in vier Phasen (Kriterien, Daten-/Problemcharakter, Baselines, Kandidaten-Shortlist) (→ [[q-vl00-essentiell|VL0]], F. 18, 30–33).

> [!warning] Kein Rezept
> Die Faustregeln sind ausdrücklich „kein generell gültiges Rezept". Sie sollen helfen, ein Gefühl für die Probleme zu entwickeln und dem Auftraggeber die richtigen Fragen zu stellen (→ VL0, F. 21).

## Intuition
Die Wahl hängt weniger vom „besten" Algorithmus ab als von Daten und Kontext: Bei wenigen Daten droht Varianz, also einfach und regularisiert; bei vielen Daten lohnt Flexibilität. Erklärbarkeit, Latenz oder Datenschutz können ein Modell ausschließen, das die beste Metrik hätte.

## Drei Leitfragen (→ VL0, F. 18–20)
1. **Problem verstehen und klassifizieren:** Wofür? Klassifikation, Regression, Clustering oder Dimensionsreduktion?
2. **Datenlage:** gepaart/ungepaart (supervised, unsupervised, self-supervised, reinforcement)? Menge (Verhältnis N zu p)? Qualität (Rauschen, Fehler)? Typ/Struktur (zeitlich, tabellarisch, Netzwerk; 1D Zeitreihen/Audio, 2D Bilder, 3D Videos/Punktwolken)?
3. **Zusatzanforderungen:** Erklärbarkeit, Genauigkeit, Performance, Compute-Ressourcen.

## Faustregeln nach Datenmenge (→ VL0, F. 22–24)
| Datenmenge | Fokus | Gute Default-Modelle | Faustregeln |
|---|---|---|---|
| **wenig** (grob N < 1 000 bzw. N < p) | Varianz klein halten, Overfitting vermeiden | lineare/logistische Regression (oft regularisiert), SVM, kleine Entscheidungsbäume | lieber einfach und regularisiert; viel CV (5–10-fold); Feature Engineering bringt oft mehr als Modellkomplexität |
| **mittel** (grob 1 000 < N < 100 000) | nichtlineare Effekte **oder** Interpretierbarkeit | Gradient Boosted Trees (tabellarisch), Random Forest, regularisierte lineare Modelle als Vergleich | tabellarisch fast immer GBT probieren; bei Interpretierbarkeit lin./log. Regression + Regularisierung; bei großem/korreliertem p Feature-Selektion oder Regularisierung |
| **groß** (grob N > 100 000) | Varianz weniger kritisch, Flexibilität lohnt | GBT (tabellarisch), neuronale Netze (Deep/Transfer Learning), SGD für große lineare Probleme | Text/Bild: vortrainierte Modelle finetunen; tabellarisch NN nur mit gutem Grund |

**Verhältnis p zu N** (→ VL0, F. 25): p groß, N klein → lineare Modelle, starke Regularisierung, Feature-Selektion. N ≫ p → komplexere Modelle weniger riskant.

**Datentyp** (→ VL0, F. 26): tabellarisch → GBDT als Startpunkt; Text → Transformer bei viel Daten, Transfer Learning bei wenig; Bild/Audio → Transfer Learning (außer sehr spezielle Daten); Zeitreihen → simpel starten, Deep Learning erst bei viel Daten/komplexen Mustern.

**Rauschen/Label-Qualität** (→ VL0, F. 27): Komplexere Modelle helfen bei starkem Rauschen meist nicht; eher robuste Modelle, Regularisierung, bessere Labels/Features.

**Interpretierbarkeit** (→ VL0, F. 28): Erklärbarkeit, Stabilität, geringe Latenz → kleinere (lineare) Modelle; Accuracy im Vordergrund mit genug Daten und Compute → komplexere Modelle.

**Zusätzliche Fragen** (→ VL0, F. 29): Gibt es Lösungen für ähnliche Probleme? Ein (approximatives) physikalisches Modell? Klassische Methoden, die das Lernproblem vereinfachen? Müssen Daten bereinigt werden?

## Vier Phasen (→ VL0, F. 30–33)
| Phase | Frage | Tasks | Output |
|---|---|---|---|
| **1 Entscheidungskriterien** | Woran wird das Modell gemessen? | Problemtyp festlegen; Fachziel in messbare Zielgröße übersetzen; Einsatzkontext (Batch vs. Echtzeit, Latenz, Hardware, Budget); Constraints (Datenschutz, Fairness, Erklärbarkeit, Wartbarkeit); Abnahmekriterien (Muss/Kann) | 1–2 Absätze: Problemdefinition, klare Metrik, Constraints, Abnahmekriterien |
| **2 Daten-/Problemcharakter** | Welche Modellfamilien passen grundsätzlich? | Datentyp; Größe (N, p); Qualität (Rauschen, Class Imbalance, schlechte Labels) | Kurzprofil: Problemklasse, Datenbeschreibung, geeignete Modellklassen |
| **3 Baselines** | Woran messen wir, ob Komplexität etwas bringt? | Dummy-Baseline (Mittelwert, Schwellwert …); ML-Baseline (lineare/logistische Regression) | Baseline-Spezifikation, Vermutungen zur Verbesserung |
| **4 Kandidaten-Shortlist** | 2–4 Modellfamilien statt 20 Varianten | Faustregeln anwenden; Literatur zu ähnlichen Problemen; Abgleich mit Anforderungen | Shortlist mit Methodenbeschreibung und Begründung |

**Danach** (→ VL0, F. 34): fairer Vergleich mit identischem Evaluierungsprotokoll; erst grob suchen, welche Familie gewinnt, dann feintunen; Fehleranalyse (Bias vs. Varianz, Fehler ansehen); finale Entscheidung als Gesamtpaket, nicht nur nach bester Metrik; zuerst simpel und schnell (Downsampling, Teildaten), dann erweitern.

## Annahmen & Voraussetzungen
- Die Datengrenzen (1 000, 100 000) sind grobe Orientierungswerte.

## Beispiel
Zwei eigene Workshop-Fälle (Fake-Shop-Erkennung, Qualitätskontrolle in der Produktion) gegen die vier Phasen gespiegelt: [[modellauswahl-fallbeispiele]].

> [!beispiel] Rechenbeispiel (nicht aus der VL) – warum Phase 3 eine Dummy-Baseline braucht
> 1 000 Kredite, davon 50 Ausfälle (5 %).
> - **Dummy** „nie Ausfall": Accuracy $= 950/1000 = 95\,\%$, findet aber keinen einzigen Ausfall (Recall 0).
> - **Modell:** TP = 30, FN = 20, FP = 40, TN = 910 ⇒ Accuracy $= 940/1000 = 94\,\%$, Recall $= 30/50 = 60\,\%$, Precision $= 30/70 \approx 43\,\%$.
>
> Nach Accuracy ist das Modell schlechter als die Dummy-Baseline, fachlich aber viel nützlicher. Deshalb muss in **Phase 1** eine zum Fachziel passende Metrik festgelegt werden (hier Recall bei tragbarer Precision), sonst wählt man das falsche Modell.

> [!beispiel] Anwendungsfall (nicht aus der VL) – Stromverbrauch pro Haushalt für morgen
> 1. **Kriterien:** Regression; Metrik MAE in kWh; Prognose nächtlich im Batch (Latenz unkritisch); Constraint Datenschutz (Smart-Meter-Daten); Abnahme z. B. MAE mindestens 15 % besser als Dummy.
> 2. **Daten:** tabellarisch mit Zeitbezug; N groß (200 000 Haushalte × viele Tage), p ≈ 30 (Wetter, Wochentag, Feiertag, Vortagsverbrauch); einzelne Messlücken.
> 3. **Baselines:** Dummy = Verbrauch am gleichen Wochentag der Vorwoche; ML-Baseline = lineare Regression mit Wetter und Wochentag.
> 4. **Shortlist:** Gradient Boosted Trees (tabellarisch, viel Daten), regularisierte lineare Regression (erklärbar, Vergleich); ein neuronales Netz nur mit gutem Grund (VL0, F. 24).
>
> Validierung **zeitlich** getrennt (trainieren auf Vergangenheit, testen auf späteren Wochen), sonst Leakage ([[train-val-test-split]]).

## Typische Prüfungsfragen & Fallstricke
- **Fallbeschreibung → Modellvorschlag mit Begründung**: Datentyp, N, p/N, Rauschen, Interpretierbarkeit nennen, dann 2–4 Kandidaten plus Baseline.
- „Warum braucht man eine Dummy-Baseline?" → Ohne sie sieht man nicht, ob ein Modell überhaupt etwas lernt (Beispiel Accuracy-Falle in [[churn-output-interpretation]]).
- Fallstrick: nur die beste Metrik betrachten statt Gesamtpaket (Erklärbarkeit, Latenz, Wartbarkeit, Fairness).
- Fallstrick: bei unbalancierten Klassen Accuracy als Metrik wählen.

> [!ergaenzung] Klausurhinweis (nicht aus der VL) – laut [[q-lernhilfe-sl2|Lernhilfe]], S. 7
> Ein beliebtes Klausurmuster: Szenario vorgeben, Methode empfehlen und begründen – etwa 500 Patienten, 2000 Gen-Merkmale, Erklärbarkeit nötig. Denkweg: Problemtyp → N vs. p → Datentyp → Anforderungen, und immer mit einer Baseline beginnen. Ausgearbeitet als [[modellauswahl-fallbeispiele#Fall C · Genexpression|Fall C]].

## Verwandt
- [[bias-varianz-tradeoff]] – Hintergrund der Faustregeln
- [[regularisierung]] – „lieber einfach und regularisiert"
- [[kreuzvalidierung]] – fairer Vergleich, viel CV bei wenig Daten
- [[transparente-modelle]] – wenn Interpretierbarkeit eine Anforderung ist
- [[fairness-grundlagen]] – Fairness als Constraint in Phase 1

## Karteikarten
Welche drei Leitfragen stellt die VL zur Methodenwahl?::Problem verstehen/klassifizieren, Datenlage prüfen (Menge, Qualität, Typ, Paare), Zusatzanforderungen (Erklärbarkeit, Genauigkeit, Performance, Compute).
Gute Default-Modelle bei wenig Daten (N < 1000 bzw. N < p)?::Lineare/logistische Regression (regularisiert), SVM, kleine Entscheidungsbäume.
Gute Default-Modelle bei mittlerer Datenmenge (1000–100000), tabellarisch?::Gradient Boosted Trees, Random Forest, regularisierte lineare Modelle als Vergleich.
Was empfiehlt die VL bei p groß und N klein?::Lineare Modelle, starke Regularisierung, Feature-Selektion.
Was empfiehlt die VL für Text/Bild bei großen Datenmengen?::Vortrainierte Modelle (Transfer Learning) finetunen.
Wie heißen die vier Phasen der Modellauswahl?::1 Entscheidungskriterien, 2 Daten-/Problemcharakter, 3 Baselines, 4 Kandidaten-Shortlist.
Welche zwei Baselines nennt die VL?::Dummy-Baseline (z. B. Mittelwert, Schwellwert) und ML-Baseline (lineare/logistische Regression).
Wie viele Modellfamilien gehören in die Shortlist?::2–4 Modellfamilien statt 20 Varianten.
Was hilft bei stark verrauschten Labels laut VL?::Robuste Modelle, Regularisierung, bessere Labels/Features – nicht ein komplexeres Modell.

## Quellen
- [[q-vl00-essentiell|VL0]], Folien 17–34
- [[q-vl02-bias-varianz-modellwahl|VL2]], Folien 9–27
- [[q-lernhilfe-sl2|Lernhilfe]], S. 5–7 (Klausurhinweis)
