---
typ: methode
themen: [xai, interpretierbarkeit]
aliases: [Transparente Modelle, Interpretierbare Modelle, Scorecards, Regellisten, Transparenzspektrum, Koeffizienten als Erklärung]
quellen: ["[[q-vl05-xai-transparente-globale-modelle]]"]
relevanz: mittel
beherrschung: neu
aktualisiert: 2026-10-15
---

# Transparente Modelle

#flashcards/statistik

## Kurzdefinition
Modelle, deren **Struktur selbst die Erklärung trägt** – Koeffizienten, Pfade und Regeln lassen sich direkt lesen. Eine transparente Baseline ist nicht nur ein Benchmark, sondern oft das **erste Audit-Artefakt** (→ [[q-vl05-xai-transparente-globale-modelle|VL5]], F. 9–10).

## Intuition
Bevor man ein komplexes Modell nachträglich erklärt, sollte man fragen, ob ein verständliches Modell reicht. Bei Entscheidungen mit hohen Auswirkungen rechtfertigen kleine Performancegewinne möglicherweise keine großen Verluste an Prüfbarkeit (→ VL5, F. 10).

## Transparenzspektrum (→ VL5, F. 10)
| Transparenz | Modelle |
|---|---|
| sehr transparent | Scorecards, dünn besetzte lineare Modelle, flache Entscheidungsbäume |
| mittel | regularisierte GLMs, einfache Priors, regelbasierte Modelle |
| schwer direkt zu untersuchen | Random Forests, Gradient Boosting, tiefe neuronale Netze |

**Leitfragen** (→ VL5, F. 9): Wie können Koeffizienten, Pfade und Regeln zur Erklärung dienen? Wie verändern Regularisierung und Nebenbedingungen die Interpretierbarkeit? Wann ist eine transparente Baseline einem komplexeren Modell vorzuziehen?

## Lineare Modelle als Erklärung (→ VL5, F. 11)
Modell $y = \beta_0 + \beta_1x_1 + \dots + \beta_px_p$:
- Jeder Koeffizient ist eine **partielle Assoziation**, bedingt auf die einbezogenen Merkmale (nicht kausal).
- Bei **standardisierten** Merkmalen sind Koeffizientenbeträge besser vergleichbar; bei Rohmerkmalen bestimmen die Einheiten die Größe.
- **Konfidenz- oder Bootstrap-Intervalle** zeigen, ob Effekte stabil genug zur Diskussion sind ([[bootstrap]]).
- **Multikollinearität** erhöht die Unsicherheit und kann Vorzeichen kippen → Korrelationen vor der Interpretation prüfen.
- Lineare Koeffizienten sind **additiv**; Interaktionen müssen explizit modelliert werden, wenn der Effekt von $x_1$ von $x_2$ abhängt.

## Regularisierung und Interpretierbarkeit (→ VL5, F. 13)
- **L1** $\lambda\sum\lvert\theta_j\rvert$ → sparse Gewichte, einige exakt 0 ([[lasso-regression]]).
- **L2** $\lambda\sum\theta_j^2$ → verkleinert gleichmäßig, verbessert die Stabilität ([[ridge-regression]]).
- **Elastic Net** → wenn Gruppen korrelierter Variablen stabilisiert werden sollen.
- Penalty-Stärke per **Cross-Validation** bestimmen, nicht durch Wahl der besten Resultate oder „Geschichte".
- Nach der Feature-Selektion Performance, Fairness-Metriken und Koeffizientenstabilität **erneut prüfen**.

## Scorecards und Regellisten (→ VL5, F. 15)
- **Scorecards** wandeln gebinnte Merkmalswerte in Punkte um; die Gesamtpunktzahl wird mit einem Schwellenwert verglichen.
- **Regellisten** sind geordnete Wenn-dann-Regeln; die erste passende Regel bestimmt häufig die Vorhersage.
- leicht auditierbar – jede Entscheidung ist auf wenige Bedingungen zurückführbar.
- **Technische Risiken:** Informationsverlust durch Binning, Threshold-Gaming, **versteckte Proxys** (ein Merkmal „springt" für ein geschütztes Attribut ein, obwohl dieses nicht im Modell ist).
- Validierung: Kalibrierung, Subgruppenleistung, Plausibilität jedes Bins/jeder Regel.

## Zusammenfassung (→ VL5, F. 16)
- Auch transparente Modelle brauchen sorgfältige Interpretation: Skala, Korrelation, Unsicherheit, Annahmen.
- Lineare und logistische Regression sind starke Baselines, wenn ihre Annahmen präzise kommuniziert werden.
- Bäume, Regeln und Scorecards haben nachvollziehbare Entscheidungswege, können aber instabil sein oder problematische Schwellen kodieren.
- GLMs und Nebenbedingungen sind ein Mittelweg zwischen Flexibilität und Untersuchbarkeit.

## Typische Prüfungsfragen & Fallstricke
- Transparenzspektrum mit je zwei Beispielen.
- „Wie interpretiert man einen Koeffizienten korrekt?" → partielle Assoziation, bedingt auf die anderen Merkmale, Einheiten beachten, nicht kausal.
- Fallstrick: Koeffizienten unstandardisierter Merkmale nach Größe vergleichen.
- Fallstrick: Multikollinearität ignorieren.

## Verwandt
- [[logistische-regression]] – Log-Odds und Odds Ratios
- [[entscheidungsbaeume]] – Pfade als Erklärung
- [[erklaerbarkeit-grundlagen]] – Taxonomie
- [[fairness-grundlagen]] – Proxys
- [[modellauswahl-vorgehen]] – Interpretierbarkeit als Anforderung

## Karteikarten
Nenne drei sehr transparente Modellarten.::Scorecards, dünn besetzte lineare Modelle, flache Entscheidungsbäume.
Was bedeutet ein linearer Koeffizient technisch?::Eine partielle Assoziation, bedingt auf die einbezogenen Merkmale – kein kausaler Effekt.
Warum Merkmale vor dem Vergleich von Koeffizienten standardisieren?::Sonst bestimmen die Einheiten die Größe der Koeffizienten.
Was bewirkt Multikollinearität bei der Interpretation?::Höhere Unsicherheit, Vorzeichen können kippen.
Was ist ein versteckter Proxy?::Ein Merkmal, das für ein geschütztes Attribut einspringt, obwohl dieses nicht im Modell ist.
Drei technische Risiken von Scorecards?::Informationsverlust durch Binning, Threshold-Gaming, versteckte Proxys.
Warum ist eine transparente Baseline mehr als ein Benchmark?::Sie ist oft das erste Audit-Artefakt.

## Quellen
- [[q-vl05-xai-transparente-globale-modelle|VL5]], Folien 9–16
