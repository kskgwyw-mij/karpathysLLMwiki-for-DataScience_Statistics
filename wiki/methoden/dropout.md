---
typ: methode
themen: [regularisierung, neuronale-netze]
aliases: [Dropout, Dropout-Rate, MC-Dropout]
quellen: ["[[q-vl00-essentiell]]", "[[q-vl03-regularisierung-cv]]", "[[q-nb02-mnist-regularization]]"]
relevanz: hoch
beherrschung: neu
aktualisiert: 2026-10-15
---

# Dropout

#flashcards/statistik

## Kurzdefinition
Beim Dropout werden Bestandteile des Modells (z. B. Neuronen) während der Optimierung **zufällig weggenommen**. Bei der Auswertung des trainierten Modells werden **alle** Bestandteile verwendet (→ [[q-vl00-essentiell|VL0]], F. 51).

## Intuition
Kein Neuron kann sich darauf verlassen, dass ein bestimmtes anderes Neuron da ist – die Repräsentation wird redundanter und robuster. Dropout lässt sich als **Ensemble** vieler Teilmodelle sehen, die sich die meisten Parameter teilen (→ VL0, F. 51), ähnlich zu [[bagging]].

## Formal
- Training: jedes Element wird mit Wahrscheinlichkeit $p$ (Dropout-Rate) auf 0 gesetzt.
- Inferenz: alle Elemente aktiv. In PyTorch über den Modus: `model.train()` → `model.eval()` (die Folie schreibt `model.test`).
- Spezialfall: Dropout **auch in der Inferenz**, um eine **Konfidenz** der Vorhersage zu schätzen (→ VL0, F. 51).

> [!info] Ergänzung (nicht aus der VL)
> Damit die erwartete Aktivierung gleich bleibt, skalieren Implementierungen im Training die verbleibenden Werte mit $1/(1-p)$ („inverted dropout"). Wiederholte stochastische Vorhersagen mit aktivem Dropout heißen MC-Dropout.

## Praxisempfehlungen (→ [[q-vl03-regularisierung-cv|VL3]], F. 30)
| | |
|---|---|
| Fully-connected | Rate 0,2–0,5 |
| Convolutional | eher kleiner, 0,1–0,3 |
| Kombination | mit L2-Regularisierung |
| Höhere Rate | stärkere Regularisierung |
| Tiefere Schichten | eventuell kleinere Werte |
| Kleine Datensätze | nicht zu hoch |

**Fallstricke:** zu hohe Rate → Lernverlangsamung, hoher Bias, degradierte Repräsentationen; in kleinen Netzen kann L2- oder Design-Regularisierung effektiver sein als starkes Dropout (→ VL3, F. 30).

## Beispiel
[[q-nb02-mnist-regularization|NB2]], Dropout 0,5 im kleinen MLP (64 Hidden): Test-Accuracy 0,921 gegenüber 0,924 der Baseline – bei kleinem Netz und nur 8 Epochen bringt starkes Dropout nichts, passend zum Fallstrick oben.

## Typische Prüfungsfragen & Fallstricke
- „Warum wirkt Dropout regularisierend?" → Ensemble mit geteilten Parametern, keine Co-Adaption.
- „Was ändert sich zwischen Training und Inferenz?" → Training zufälliges Weglassen, Inferenz alles aktiv (außer für Unsicherheitsschätzung).
- Fallstrick: vergessen, das Modell in den Auswertungsmodus zu schalten.

## Verwandt
- [[regularisierung]] – Taxonomie
- [[bagging]] – Ensemble-Idee
- [[label-smoothing]] – weitere Regularisierung für Netze
- [[ridge-regression]] – empfohlene Kombination

## Karteikarten
Was passiert beim Dropout?::Im Training werden Modellbestandteile zufällig weggelassen; in der Auswertung werden alle verwendet.
Als was kann man Dropout interpretieren?::Als Ensemble mehrerer Modelle, die viele Parameter teilen.
Typische Dropout-Rate für Fully-connected bzw. Conv-Layer?::FC 0,2–0,5; Conv 0,1–0,3.
Wozu Dropout in der Inferenz?::Um eine Konfidenz der Vorhersage zu schätzen (Spezialfall).
Risiko einer zu hohen Dropout-Rate?::Lernverlangsamung, hoher Bias, degradierte Repräsentationen.

## Quellen
- [[q-vl00-essentiell|VL0]], Folie 51
- [[q-vl03-regularisierung-cv|VL3]], Folien 29–30
- [[q-nb02-mnist-regularization|NB2]]
