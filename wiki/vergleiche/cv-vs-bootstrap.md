---
typ: vergleich
themen: [resampling]
aliases: [Cross-Validation vs Bootstrap]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl04-map-resampling-optimierung]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Vergleich: Cross-Validation vs. Bootstrap

#flashcards/statistik

| | [[kreuzvalidierung\|Cross-Validation]] | [[bootstrap\|Bootstrap]] |
|---|---|---|
| Ziehen | ohne Zurücklegen, disjunkte Folds | **mit** Zurücklegen, $n$ aus $n$ |
| Testdaten | jeweils ein Fold | Out-of-Bag (ca. 36,8 %) |
| Hauptzweck | Generalisierungsfehler schätzen, **Modellauswahl**, Tuning | **Unsicherheit** (Standardfehler, Konfidenzintervalle), Stabilität, Bagging |
| Wiederholungen | $K$ (z. B. 5–10) bzw. $N$ (LOOCV) | $B$ (oft Hunderte bis Tausende) |
| Output | Mittelwert der Fold-Fehler | Verteilung $\{s^b\}$ einer Statistik |
| Empfehlung der VL | für Modellauswahl oft bevorzugt (→ VL0, F. 67) | OOB-Evaluation „bequem" |

Gemeinsam: Die **gesamte Pipeline** gehört in jede Wiederholung; Resampling muss Datenstruktur und Leakage-Risiken respektieren (→ [[q-vl00-essentiell|VL0]], F. 64–65).

## Karteikarten
Hauptunterschied im Ziehen zwischen CV und Bootstrap?::CV teilt ohne Zurücklegen in disjunkte Folds; Bootstrap zieht $n$ aus $n$ mit Zurücklegen.
Was wird für die Modellauswahl laut VL oft bevorzugt, CV oder OOB-Bootstrap?::Cross-Validation.
Wofür ist der Bootstrap besonders geeignet?::Unsicherheitsquantifizierung (Standardfehler, Konfidenzintervalle), Stabilitätsanalysen, Bagging.

## Verwandt
- [[kreuzvalidierung]]
- [[bootstrap]]
- [[train-val-test-split]]
